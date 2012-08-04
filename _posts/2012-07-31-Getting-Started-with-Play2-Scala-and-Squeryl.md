---
layout: post
title: 【意訳】Scala on Play2 with Squerylではじめるデータベース駆動アプリ　#play_ja
tags: Play2 Heroku Database-Driven Scala Squeryl CoffeeScript JSON jQuery ScalaTest
categories: Programing
---
【意訳】Scala on Play2 with Squerylではじめるデータベース駆動アプリ
-----------------

この記事は、HerokuのPrincipal Developer EvangelistのJames Ward氏とRyan Knight氏の記事の意訳です。参考程度にどうぞ。

![Getting Started with Play 2, Scala, and Squeryl](http://capture.heartrails.com/300x200/cool?http://www.artima.com/articles/play2_scala_squeryl.html)

誤訳・誤植等ありましたら、[@modal_soul](https://twitter.com/modal_soul)までリプライいただけるとありがたいです。
<hr />

## サマリー
<table border="1"><tr><td>
この記事では、Play2,Scala,Squeryl,JSON,CoffeScript,CoffeScript,jQueryとScalaTestを使ったWebアプリケーションの作り方を紹介します。また、同時にScalaTestを使ったテスト方法とHerokuを使ったクラウド環境へのアプリケーションデプロイについても学べます。
</td></tr>
</table>


Play2,Scala,Squeryl,JSON,CoffeScript,jQueryとScalaTestの組み合わせによって、モダンなWebアプリケーションの構築とテストを効率的に行うことができ,
シンプルなデータベース駆動Webアプリケーションの構築、テスト、クラウドへのデプロイを体験できます。


## Play2の紹介
Play2は数あるWebフレームワークの中でも、ステートレスアーキテクチャで設計された、ユニークなWebフレームワークです。
Play2は、メモリやCPUなどのリソースの消費を抑えた、とても軽量なフレームワークです。
ステートレスであるため、昨今クラウドアーキテクチャにとって重要な、水平のスケーリングを容易に行うことができます。
他のアーキテクチャの特徴としては、完全に非同期なHTTPプログラミングモデルを使った、Websocketやcometのようなlong lived-connectionを利用するために設計されていることです。


また他の特徴としては、Herokuのようなクラウドアプリケーションプラットフォームへのデプロイに適するように、完璧に完結していることです。
Playアプリケーションのデプロイはシンプルで、コンテナベースのアプローチをしてきたJava開発者の多くが経験してきた環境齟齬を回避することができます。

Ruby on Rails, GrailsやDjangoなどの新しいWebフレームワークは、動的型付によってとても優れた生産性を発揮します。Playも動的言語フレームワークが持っているのと同じように、素早い繰り返しと高速な開発を行うことができ、しかしながらJavaやScalaのような静的型付言語による恩恵も留めています。これによりコンパイラは生産性を妨害することなく開発者を支援することができます。


## Squerylの紹介
データベースとのインテグレーションに最適の技術は何か？これは今まで幾度と無く討論されてきました。幸いなことに、JavaやScalaのエコシステムでは、データの永続化の方法が複数用意され、選択することができます。
Play2のデフォルトのデータベースマッピングツールはAnormが使われています。(※AnormはORMではありません)
名前が指し示す通り、Anormはオブジェクトとリレーショナルモデルとの自動マッピングを行いません。
代わりに、開発者はネイティブなSQLを書き、手動でリレーショナルデータとオブジェクトのマッピングを行います。
この手法では、実行される生のクエリーをチューニングすることができるという利点があります。
Playの開発者は、SQLを使用することはリレーショナルデータベースとの対話とSQL層を抽象化することにおいての優れたDSLであり、パワフルで柔軟な方法だと主張してます。


Squerylは、Scalaにおけるデータ永続化のためのAnormの代替手段です。Anormと比較して、SquerylはHibernateに似ており、オブジェクトリレーショナルマッピングを提供します。Squerylはデータベースとの対話において、型安全なDSLを提供します。
またSquerylは、明示的に取得されるデータオブジェクトの粒度を制御することができます。
Hibernateのような従来のORMと共通のN+1問題に対するエレガントなソリューションを提供します。


SquerylはPlay2のデフォルトのデータ永続化ライブラリではありませんが、特別な追加設定やセットアップは必要としません。データベースエボリューションは手動で行う必要があり、データベース接続は初期起動時に生成する必要があります。トランザクションも同様に、コントローラーで呼ばれた際に、明示的に定義される必要があります。

## Play2でのSquerylのセットアップ
Play2プロジェクトがまだない場合、Play2をインストール後、新規プロジェクトを作成しましょう。

{% highlight sh %}
play new mysquerylapp
{% endhighlight %}


プロジェクトの言語はScalaを選択しましょう。

SquerylライブラリをPlayプロジェクトに追加する必要があります。後ほどプロジェクトはHerokuにデプロイします。HerokuのデフォルトのデータベースはPostgreSQLで、PostgreJDBCドライバーも依存関係にあるためここで追加します。

project/Build.scalaファイルを編集し、依存関係を更新しましょう。

{% highlight sh %}
val appDependencies = Seq(
  "org.squeryl" %% "squeryl" % "0.9.5-2",
  "postgresql" % "postgresql" % "9.1-901-1.jdbc4"
)
{% endhighlight %}


IDEにEclipseやIntellijを使っている場合、Playはプロジェクトを自動作成してくれます。

Intellijの場合
{% highlight sh %}
play idea
{% endhighlight %}


Eclipseの場合
{% highlight sh %}
play eclipsify
{% endhighlight %}


ここでは注意ですが、プロジェクトファイルは依存関係を更新した後に作成しましょう。なぜならプロジェクトは必要なライブラリも含めて構成されるからです。もし後で依存関係を更新する場合は、プロジェクトを生成するコマンドを再度実行してください。


これでプロジェクトを実行することができます。プロジェクトルートに移動し、以下のコマンドを実行しましょう。

{% highlight sh %}
play run
{% endhighlight %}


サーバは以下のURLで起動しています。確かめてみましょう。

{% highlight sh %}
http://localhost:9000
{% endhighlight %}


ローカルでのテストではインメモリのh2データベースを使用します。Playでこのデータベースを使用するには、conf/application.confファイルを修正し、以下の行のコメントアウトの解除か、行の追加をします。

{% highlight sh %}
db.default.driver=org.h2.Driver
db.default.url="jdbc:h2:mem:play"
{% endhighlight %}


最後のセットアップ手順として、Squerylデータベースを接続します。データベースの接続情報を取得するために、Play標準の構成システムを使います。
Playアプリケーションのライフサイクルのスタートアップフェーズにフックするグローバルクラスを追加するだけで完了です。
app/Global.scalaファイルを新規に作成し、以下を記述します。

{% highlight sh %}
import org.squeryl.adapters.{H2Adapter, PostgreSqlAdapter}
import org.squeryl.internals.DatabaseAdapter
import org.squeryl.{Session, SessionFactory}
import play.api.db.DB
import play.api.GlobalSettings

import play.api.Application

object Global extends GlobalSettings {

  override def onStart(app: Application) {
    SessionFactory.concreteFactory = app.configuration.getString("db.default.driver") match {
      case Some("org.h2.Driver") => Some(() => getSession(new H2Adapter, app))
      case Some("org.postgresql.Driver") => Some(() => getSession(new PostgreSqlAdapter, app))
      case _ => sys.error("Database driver must be either org.h2.Driver or org.postgresql.Driver")
    }
  }

  def getSession(adapter:DatabaseAdapter, app: Application) = Session.create(DB.getConnection()(app), adapter)

}
{% endhighlight %}


アプリケーションスタートアップのdb.default.driver設定パラメータは、データベース接続のセットアップのために使用するドライバを決定するために使われます。新しいコネクションが生成され、SquerylのSessionFactoryにストアされます。

ブラウザで、http://localhost:9000をリロードすると、アプリケーションが動作し、PlayのSTDOUTログで以下のメッセージが表示されるはずです。

{% highlight sh %}
[info] play - database [default] connected at jdbc:h2:mem:play
{% endhighlight %}


## Entityの作成
それでは、データベースでデータを保持するためのシンプルなEntityオブジェクトを作成しましょう。app/models/Bar.scalaファイルを新規に作成し、以下のを記述します。

{% highlight sh %}
package models

import org.squeryl.{Schema, KeyedEntity}

case class Bar(name: Option[String]) extends KeyedEntity[Long] {
  val id: Long = 0
}

object AppDB extends Schema {
  val barTable = table[Bar]("bar")
}
{% endhighlight %}


これはBarオブジェクトのリストを保持するかなりシンプルなentityです。各々のBarはnameとプライマリキーとしてIDを持っています。Scalaのcase classは,不変で、基本的にクラスを文法上の便宜で満たすものです。また、クライアントから返却されたフォーム値のマッチングを行う際にとても便利なパターンマッチングとして使用することもできます。AppDBオブジェクトは、Squerylがデータベースへマッピングするスキーマのインスタンスです。今回の場合、データベースにbarというテーブルを１つ定義します。スキーマは単一のオブジェクトとして宣言されるため、シングルトンオブジェクトを生成します。


Squerylを使うと、AppDB.createメソッドを呼ぶだけでプラグラムチックにデータベーススキーマを生成することができます。ですが、手動でSQLスクリプトを作成する方法をお勧めします（PlayではこのSQLスクリプトをevolutions scriptと呼びます)。Playは、これらのSQLスクリプトに対応するデータベーススキーマをチェックして、データベーススキーマの変更を追跡します。Playは、スキーマが古くなっていることを検出すると、このSQLスクリプトの適用を提案してきます。これはDEVモードのときにのみ行われ、PRODモードでは、アプリケーションの起動前にスクリプトを適用されます。これにより、データベーススキーマの変更やバージョンのロールバックを行う必要がある場合に、スキーマの変更をコントロールすることができます。


conf/evolutions/default/1.sqlファイルを新規作成して、以下を記述します。
{% highlight sh %}
# --- First database schema

# --- !Ups

create sequence s_bar_id;

create table bar (
  id    bigint DEFAULT nextval('s_bar_id'),
  name  varchar(128)
);


# --- !Downs

drop table bar;
drop sequence s_bar_id;
{% endhighlight %}


このSQLファイルは”Ups”と”Downs”の2つのセクションから成っています。”Ups”セクションは、データベーススキーマに変更を反映させ、”Downs”セクションは、データベースへの変更を取り消す際の記述です。Playは、データベーススキーマへの変更をこれらのファイルの名前順に適用します。もし、デプロイ後1.sqlの適用後にスキーマへ変更を行う必要がある場合は、2.sqlファイルに変更内容を記述します。
http://localhost:9000をリロードすると、Playがデータベースへの変更を適用するか尋ねてくるはずです。1.sqlの”Ups”セクションをローカルのインメモリデータベースに適用するために、Apply this script now!ボタンをクリックします。


## モデルのテスト





『まだ途中です』




