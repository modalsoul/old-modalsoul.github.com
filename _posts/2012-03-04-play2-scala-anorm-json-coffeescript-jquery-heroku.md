---
layout: post
title: 【意訳】Tutorial：Play Framework 2 with Scala, Anorm, JSON, CoffeeScript, jQuery & Heroku [Herokuデプロイ編]
tags: Play2 Scala CoffeeScript jQuery Heroku
categories: Programing
---
【意訳】Tutorial: Play Framework2 with Scala, Anorm, JSON, CoffeeScript, jQuery & Heroku　[Herokuデプロイ編]
-----------------

元記事は[こちら](http://www.jamesward.com/2012/02/21/play-framework-2-with-scala-anorm-json-coffeescript-jquery-heroku)

少々長い記事なので、Play2アプリのビルド編とHerokuへのデプロイ編に分けます。

今回は、Herokuへのデプロイ編です。

Play2アプリのビルド編は、[こちら](http://modalsoul.github.com/Programing/2012/02/29/play2-scala-anorm-json-coffeescript-jquery-heroku/)

![Play2アプリビルド編](http://capture.heartrails.com/200x150/cool?http://modalsoul.github.com/Programing/2012/02/29/play2-scala-anorm-json-coffeescript-jquery-heroku/)
<hr />

Step 1) [Herokuのアカウントを取得](http://heroku.com/signup)して、[Heroku Toolbelt](http://toolbelt.heroku.com/)と[git](http://git-scm.org/)をインストールして、コマンドラインからHerokuへログインします

{% highlight sh %}

$ heroku login

{% endhighlight %}

もしこれがはじめてのHerokuへのログインだったら、git用のSSHキーの作成と、Herokuアカウントへの紐付けをする必要があります。

Step 2) Heroku上の各アプリケーションは、テスト用にPostgres DBを持っています。
Heroku上でアプリケーションを実行してこのDBを使うには、別途設定が必要です。
これにはいくつか方法がありますが、最も簡単な方法はスタートアップコマンドラインからDBの設定をオーバーライドする方法です。
これはStep3で紹介します。
その前に、Psotgres JDBCドライバーをプロジェクトの依存関係として指定する必要があります。
"project/Build.scala"ファイルに、以下のような"appDependencies"を設定します。


{% highlight sh %}

   val appDependencies = Seq(
      "postgresql" % "postgresql" % "9.1-901-1.jdbc4"
    )

{% endhighlight %}


Step 3) Herokuにどのプロセスを実行するのかを教えるために、プロジェクトルートディレクトリ配下にファイル名"Procfile"を作成し（※大文字と小文字が区別されます）、以下のように記述します

{% highlight sh %}

web: target/start -Dhttp.port=$PORT -DapplyEvolutions.default=true -Ddb.default.driver=org.postgresql.Driver -Ddb.default.url=$DATABASE_URL

{% endhighlight %}


Play2では、プロジェクトのビルドにScala Build Tool(SBT)を使います。プロジェクトがHerokuへデプロイされると、プロジェクトをコンパイルするため"sbt stage!コマンドが実行されます。
そして、プロセスがJavaのクラスパスの設定とPlayサーバーの起動をする"taget/start"スクリプトを生成します。
Herokuはアプリケーションに、Javaプロパティ"http.port"が適宜設定されるように、環境変数"PORT"を使用してリッスンすべきHTTPポートを通知します。
また、Heroku上のアプリケーションへDB（とその他のリソース）への接続文字列は、デフォルトでは環境変数介して渡されます。
環境変数のDATABASE_URLは、DBホスト、名前、ユーザ名、パスワードを含みます。
所謂"db.default.url"プロパティがその値で設定されます。
またドライバは、Postgres JDBCドライバクラスに設定されます。


Step 4) Herokuはアプリケーションのアップロード方法としてgitを使っています。
SCMツールにgitを使っていようと無かろうとHerokuへアプリケーションをアップロードするにはgitを使います。
プロジェクトのルートディレクトリでgit repoを作り、ファイルを追加してコミットします。

{% highlight sh %}

	git init
	git add Procfile app conf project public
	git commit -m init

{% endhighlight %}


※ 選択しての"git add"の代わりに、".gitignore"ファイルを更新することもできます。


Step 5) Heroku CLIを使用して、Heroku上に新しいアプリケーションをプロビジョニングします。
あなたが作った各アプリケーションは1月辺り、750["dyno"](http://devcenter.heroku.com/articles/dynos)時間無料です。
開発者として無料でHerokuを使用でき、スケールした分だけ支払うことになります。
コマンドライン上で、"cedar"スタックを使って新しいアプリケーションを作ります。

{% highlight sh %}

heroku create -s cedar

{% endhighlight %}

これでアプリケーションのHTTPとgitのエンドポイントを作ります。
アプリケーションで変更した名前を使うことも、自身のドメイン名を使うことも可能です。


Step 6) アプリケーションはクラウドへデプロイする準備が整いました。コマンドラインからHerokuのマスターブランチへ"git push"します。


{% highlight sh %}

git push heroku master

{% endhighlight %}

Herokuがファイル受け取ると、Play Frameworkのプレコンパイラが実行され、Herokuが"slug file"をアセンブルし、"slug"がdynoへデプロイされます。


Step 7) これで"heroku create"に続いて出力のドメインに移動する、もしくは単純に実行することで、ブラウザでアプリケーションを表示できます。

{% highlight sh %}

heroku open

{% endhighlight %}

Scala, Anorm, JSON, CoffeeScript, jQueryを使ったPlay2アプリケーションをHeroku上にデプロイできたはずです。
これよりも詳細にHerokuの使い方を知りたいときは、[Heroku Dev Center](http://devcenter.heroku.com/)を確認してください。
もし質問があったら、私に知らせてください。

このエントリーは、CoffeeScript, jQuery, Play Framework, Scalaを使ってポストされました。パーマリンクをブックマークしてください。コメントかトラックバックをよろしく。トラックバックURLは、[こちら](http://www.jamesward.com/2012/02/21/play-framework-2-with-scala-anorm-json-coffeescript-jquery-heroku/trackback/)



