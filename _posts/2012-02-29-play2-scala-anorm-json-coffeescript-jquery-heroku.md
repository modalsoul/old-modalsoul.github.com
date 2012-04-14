---
layout: post
title: 【意訳】Tutorial：Play Framework 2 with Scala, Anorm, JSON, CoffeeScript, jQuery & Heroku [Play2アプリのビルド編]
tags: Play2 Scala CoffeeScript jQuery Heroku
categories: Programing
---
【意訳】Tutorial: Play Framework2 with Scala, Anorm, JSON, CoffeeScript, jQuery & Heroku　[Play2アプリのビルド編]
-----------------

元記事は[こちら](http://www.jamesward.com/2012/02/21/play-framework-2-with-scala-anorm-json-coffeescript-jquery-heroku)

少々長い記事なので、Play2アプリのビルド編とHerokuへのデプロイ編に分けます。

今回は、Play2アプリのビルド編です。

Herokuへのデプロイ編は、[こちら](http://modalsoul.github.com/Programing/2012/03/04/play2-scala-anorm-json-coffeescript-jquery-heroku/)

![Herokuへのデプロイ編](http://capture.heartrails.com/200x150/cool?http://modalsoul.github.com/Programing/2012/03/04/play2-scala-anorm-json-coffeescript-jquery-heroku/)

※ 2012/03/06:アドバイスいただいた箇所について修正しました。

<hr />

Play Framework2 RC2がリリースされ、モダンなWebアプリケーションを構築する手法として成熟した生産的な方法になっています。

Play2とScala,Anorm,JSON,CoffeeScript,jQueryを使って迅速なアプリケーション構築を体験してみましょう。

一度ローカルでアプリケーションが動いてしまえば、あとはHerokuにデプロイするだけです。

（注意：これは[my Play1+Java turtorial](http://www.jamesward.com/2011/12/11/tutorial-play-framework-jpa-json-jquery-heroku)の、Play2＋Scala版です。）Githubからソースを入手してください。


Step 1) Play2 RC2版をダウンロード・インストールしてください。

Step 2) 新しいアプリケーションを作ります：
{% highlight sh %}

$ play new foobar

{% endhighlight %}

プロンプトの言語選択画面がでたら、Scalaを選択してください。

Step 3) 新しく作成した"foobar"ディレクトリ配下にIDEの設定ファイルを生成します。

IntelliJでは、このように実行します。

{% highlight sh %}

$ play new foobar

{% endhighlight %}

注意：ここで生成されたIMLファイルは、プロジェクトとして直接インポート可能ではありません。代わりに、モジュールなしでプロジェクトを新たに生成して、このステップで生成したIMLファイルからそのプロジェクトへモジュールをインポートする必要があります。もしわからなければ、[Play1+Scala IntelliJ article](http://www.jamesward.com/2011/07/28/setup-play-framework-with-scala-in-intellij)が参考になります。

Eclipseプロジェクト化するために、下記のコマンドを実行
{% highlight sh %}

$ play eclipsify

{% endhighlight %}


Step 4) Playサーバの起動
{% highlight sh %}

$ play run

{% endhighlight %}


正常に起動していることをテストするために、ここにアクセス[http://localhost:9000](http://localhost:9000)


Step 5) Play2 with ScalaではデフォルトでORマッパーを提供していません。代わりにデフォルトのRDBMS永続性プロバイダとなっているのがAnormです(Anormは Object Relational Mapperではありません)。このシンプルなアプリケーションでは、1つの永続的オブジェクト：Bar(ひとつのプライマリキーと名前を持っている)を持っています。Anormは、自動でのスキーマ作成を行わないので、SQLスキーマの作成/廃棄のためのスクリプトが必要です。新しいファイル"conf/evolutions/default/1.sql"を作成し、以下の内容を記述します。

{% highlight sh %}

# --- First database schema
 
# --- !Ups
 
CREATE TABLE bar (
  id                        SERIAL PRIMARY KEY,
  name                      VARCHAR(255) NOT NULL
);
 
# --- !Downs
 
DROP TABLE IF EXISTS bar;

{% endhighlight %}

Anormでは、Scalaの"case class"を永続的/CRUDインターフェースとして、値オブジェクトとシングルトンオブジェクトとして扱うことができます。"app/models/Bar.scla"というファイルにBar case classとオブジェクトを作成して、以下の内容を記述します。

{% highlight sh %}

package models
 
import play.api.db._
import play.api.Play.current
 
import anorm._
import anorm.SqlParser._
 
case class Bar(id: Pk[Long], name: String)
 
object Bar {
 
  val simple = {
    get[Pk[Long]]("id") ~
    get[String]("name") map {
      case id~name => Bar(id, name)
    }
  }
 
  def findAll(): Seq[Bar] = {
    DB.withConnection { implicit connection =>
      SQL("select * from bar").as(Bar.simple *)
    }
  }
 
  def create(bar: Bar): Unit = {
    DB.withConnection { implicit connection =>
      SQL("insert into bar(name) values ({name})").on(
        'name -> bar.name
      ).executeUpdate()
    }
  }
 
}

{% endhighlight %}

変数の"simple"は、データベースの行値をBar case classにマップする、基本的なパーサを提供します。

静的関数、"findAll"と"create"は、一般的なデータアクセスを行います。

注意点としては、"findAll"はBarへ各行を返すために、"simple" row parserを使っている点です。


Step 6) インメモリーのh2データベースを使うために、"conf/application.conf"ファイルに以下の値を設定する。

{% highlight sh %}

db.default.driver=org.h2.Driver
db.default.url="jdbc:h2:mem:play"

{% endhighlight %}

Step 7) "app/controllers/Application.scala"を下記のように更新し、BarをHTTP POSTへ変換してデータベースに反映するアプリケーションコントローラー関数を作成します。

{% highlight sh %}

package controllers
 
import play.api.data.Form
import play.api.data.Forms.{single, nonEmptyText}
import play.api.mvc.{Action, Controller}
import anorm.NotAssigned
 
import models.Bar
 
 
object Application extends Controller {
 
  val barForm = Form(
    single("name" -> nonEmptyText)
  )
 
  def index = Action {
    Ok(views.html.index(barForm))
  }
 
  def addBar() = Action { implicit request =>
    barForm.bindFromRequest.fold(
      errors => BadRequest,
      {
        case (name) =>
          Bar.create(Bar(NotAssigned, name))
          Redirect(routes.Application.index())
      }
    )
  }
 
}

{% endhighlight %}

"barForm"マップは、フォームオブジェクトへパラメータを要求し、入力データのバリデーションを行います。
静的関数の"addBar"は、リクエストと"barForm"へのリクエストパラメータのマッピングを行います。
失敗した場合は、コントローラはBadRequestを返します。
成功した場合は、Barの名前は新しい"Bar"を構築するために使われます。そしてその"Bar"はデータベースに保存され、indexページへのリダイレクトが送られます。
index関数は、Step 9で更新されるテンプレートへ"barForm"を渡すように変更されました。


Step 8) "conf/routes"ファイルに以下の行を追加して、"/addBar" URLへのPOSTリクエストを"Application.addBar"へマップする新しいルートを作成します。

{% highlight sh %}

POST	/addBar		controllers.Application.addBar

{% endhighlight %}


Step 9) フォームパラメータを受け取るために、"main"テンプレートを拡張した"app/views/index.scala.html"テンプレートを更新し、Webページ上のフォームをレンダリングするためにPlay2 form helperを使います。

{% highlight sh %}

　＠(form: play.api.data.Form[String])
 
　＠main("Welcome to Play 2.0") ｛
 
    　＠helper.form(action = routes.Application.addBar) ｛
        　＠helper.inputText(form("name"))
        ＜input type="submit"/＞
    ｝
 
｝

{% endhighlight %}


ブラウザで[http://localhost:9000](http://localhost:9000)を開き、データベース変更の適用と、フォームのテストをします。うまくできていれば、フォームを送信するとindexページへリダイレクトされるはずです。

Step 10) JSONサービスを作成し、barの全てを取得するための関数を、"app/controllers/Application.scala"ファイルに追加します。

{% highlight sh %}

  import com.codahale.jerkson.Json
 
  def listBars() = Action {
    val bars = Bar.findAll()
 
    val json = Json.generate(bars)
 
    Ok(json).as("application/json")
  }

{% endhighlight %}


新しく作成した関数は、シリアル化されたJSONとして、"Bar.findAll()"から"Bar"オブジェクトのリストを返します。

"/listBars"へのリクエストのために、下記のように新しいGET request handlerを"conf/routes"に追加します。

{% highlight sh %}

GET		/listBars		controllers.Application.listBars

{% endhighlight %}


ブラウザで[http://localhost:9000/listBars](http://localhost:9000/listBars)を開いてみてください。Step 9で作成したbarが表示されているはずです。

Step 11) jQueryを使いJSONシリアル化されたbarの取得と、それぞれのページ追加のために、新しいCoffeeScriptファイル"app/assets/javascripts/index.coffee"を作成します。内容は以下

{% highlight sh %}

$ ->
  $.get "/listBars", (data) -＞
    $.each data, (index, item) -＞
      $("#bars").append "＜li＞Bar " + item.name + "＜/li＞"

{% endhighlight %}


CoffeScriptは、"/listBars"へのGETリクエストの作成と返却値の反復、ページ(Step 12で追加します)の"bars"エレメントへの各アイテムの追加のために、jQueryを使います。


Step 12) CoffeeScriptのソースから自動コンパイルされたindex.jsスクリプトを使うために、"app/views/index.scala.html"を以下の行を"main"ブロックへ追加し、更新します。

{% highlight sh %}

　＜script src="＠routes.Assets.at("javascripts/index.js")" type="text/javascript"＞＜/script＞
 
    　＜ul id="bars"＞＜/ul＞

{% endhighlight %}


ブラウザで、[http://localhost:9000/](http://localhost:9000/)を表示し、barがページ上部に表示され、新しいbarが期待通りに追加されていることを確認してください。


これで、Scala, Anorm, JSON, CoffeeScript, jQueryを使ったPlay2アプリのビルド完了です。ここで紹介したすべてのソースコードはGitHub上にあります。あとはHerokuへデプロイするだけです。


