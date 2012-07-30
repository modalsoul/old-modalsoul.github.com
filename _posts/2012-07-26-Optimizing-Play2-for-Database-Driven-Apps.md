---
layout: post
title: 【意訳】データベース駆動アプリのための、Play2最適化について　#play_ja
tags: Play2 Heroku Database-Driven
categories: Programing
---
【意訳】データベース駆動アプリのための、Play2最適化について
-----------------

この記事は、HerokuのPrincipal Developer Evangelistの[James Ward氏の記事](http://www.jamesward.com/2012/06/25/optimizing-play-2-for-database-driven-apps)の意訳です。参考程度にどうぞ。

![Play modules Akka](http://capture.heartrails.com/300x200/cool?http://www.playframework.org/modules/akka)

誤訳・誤植等ありましたら、[@modal_soulまで](https://twitter.com/modal_soul)リプライいただけるとありがたいです。

<hr />

先週Matt Raible氏と共に、UberConfで行われたPlay vs. Grailsスマックダウンでプレゼンテーションをしました。
このセッションの目標は、同じフレームワークを用いて同じアプリケーションを作り、Play2+JavaとGrailsを比較することです。
比較のために、いくつかのベンチマークを含む評価基準を使用しました。
結果については、こちら（[Matt's session recap blog](http://raibledesigns.com/rd/entry/play_vs_grails_smackdown_at)）に詳しく掲載されています。
評価結果のプレゼンの後、これらのタイプのアプリケーションでPlay2のAkkaスレッドシステムを最適化するのに重要なことを発見しました。
Play2は、ブロッキングのコールを含まないHTTPリクエスト(i.e. 非同期)のためにout-of-the-boxに最適化されています。
Javaのほとんどのデータベース駆動アプリでは同期コールはJDBC経由で使用されるため、Play2ではこれらのリクエストタイプのためにAkkaを調整するちょっとした設定が必要です。

シンプルな例をあげてみます。時間的に現実的なデータベースブロックをシミュレートするために、このベンチマークをローカルではなくHeroku上で実行します。
["play2bars"example app](https://github.com/jamesward/play2bars)から"java-ebean"ブランチとHerokuの共有のPostgreSQLデータベースを使います。

Apache Benchを実行して、同時100クライアント接続で10000リクエストをアプリのJSONサービスへ投げるために、同じAWSリージョンにあるEC2サーバーからHerokuのmy appとして、以下のコマンドを実行しました。

{% highlight sh %}
ab -n 10000 -c 100 http://falling-dusk-7291.herokuapp.com/bars
{% endhighlight %}

最初の最適化前の結果がこちらです

{% highlight sh %}
Concurrency Level:      100
Time taken for tests:   10.272 seconds
Complete requests:      10000
Failed requests:        0
Write errors:           0
Total transferred:      1560000 bytes
HTML transferred:       510000 bytes
Requests per second:    973.53 [#/sec] (mean)
Time per request:       102.719 [ms] (mean)
{% endhighlight %}



毎秒973リクエストは悪くはありませんが、下記のようなエラーが確認できました。

{% highlight sh %}
[error] play - Cannot invoke the action, eventually got an error: Thrown(akka.pattern.AskTimeoutException: Timed out)
{% endhighlight %}


これは、Akkaが長時間ブロックされたことを意味しており、同期のコード実行を改善するようにPlay中のAkkaの設定を変え、評価を再実行しました。その結果は以下のようになりました。

{% highlight sh %}
Concurrency Level:      100
Time taken for tests:   7.274 seconds
Complete requests:      10000
Failed requests:        0
Write errors:           0
Total transferred:      1560000 bytes
HTML transferred:       510000 bytes
Requests per second:    1374.70 [#/sec] (mean)
Time per request:       72.743 [ms] (mean)
{% endhighlight %}


今回は、毎秒1374リクエストになりました。シンプルな最適化の方法は、Akkaで処理を行うスレッドを増やすして、タイムアウト値を長く設定することです。それによりスレッドが同期DBによってブロックされた際、より多くのリクエストをハンドリングするためにさらにスレッドを呼び出します。実際に使ったPlayの設定は以下です。

{% highlight sh %}
play {
 
    akka {
 
        actor {
 
            deployment {
 
                /actions {
                    router = round-robin
                    nr-of-instances = 100
                }
 
                /promises {
                    router = round-robin
                    nr-of-instances = 100
                }
 
            }
 
            retrieveBodyParserTimeout = 5 seconds
 
            actions-dispatcher = {
                fork-join-executor {
                    parallelism-factor = 100
                    parallelism-max = 100
                }
            }
 
            promises-dispatcher = {
                fork-join-executor {
                    parallelism-factor = 100
                    parallelism-max = 100
                }
            }
 
        }
 
    }
 
}
{% endhighlight %}


Akkaで使用されるスレッドの数は、CPUと"parallelism-factor"の設定値を掛け合わせた値で、"parallelism-max"が上限値です。
デフォルトの"parallelism-factor"は"1"に設定されていて、この場合スレッド数はCPU数と同じで、"parallelism-max"のデフォルトは"24"です。これらの設定について詳しくは、[Playのドキュメント](http://www.playframework.org/documentation/2.0/AkkaCore)で確認できます。


すべての設定ファイルは[こちらのplay2barsプロジェクトから](https://github.com/jamesward/play2bars/tree/java-ebean/conf)


このベンチマークはあくまで参考であって、それぞれの環境で異なってくると思います。それぞれの環境に合わせたチューニングをしてください。質問があれば是非。



