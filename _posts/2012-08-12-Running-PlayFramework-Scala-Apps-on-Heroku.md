---
layout: post
title: 【意訳】Heroku上でPlayFramework + Scalaアプリケーションを実行する
tags: Scala PlayFramework Heroku
categories: Programing
comment: この記事は、HerokuのPrincipal Developer Evangelistの<a href="http://www.jamesward.com/2011/10/19/running-play-framework-scala-apps-on-heroku" >James Ward氏の記事</a>の意訳です。参考程度にどうぞ。
thumbnail: http://www.jamesward.com/2011/10/19/running-play-framework-scala-apps-on-heroku
---

誤訳・誤植等ありましたら、[@modal_soulまで](https://twitter.com/modal_soul)リプライいただけるとありがたいです。

<hr />

[ScalaでPlay Frameworkアプリを構築](http://scala.playframework.org/)するのが最近流行っているようです。理由としては、これまでのJVMベースのWebアプリよりも簡単に構築、デプロイできることです。ScalaでPlayアプリの構築とHerokuへのデプロイをやってみましょう。

<hr />

*Step1) [Play Framework](http://www.playframework.org/download)をインストールします（バージョン1.2.3以上をインストールしてください)。

<hr />

*Step2) Play Scalaモジュールをインストールします。

{% highlight sh %}
play install scala
{% endhighlight %}

<hr />

*Step3) ScalaサポートしたPlayアプリを作成します。

{% highlight sh %}
play new playwithscala --with scala
{% endhighlight %}


<hr />

*Step4) アプリケーションをスタートします。

{% highlight sh %}
cd playwithscala
play run
{% endhighlight %}

<hr />


*Step5) ブラウザでアプリケーションを開きます。

{% highlight sh %}
http://localhost:9000
{% endhighlight %}


とても簡単ですね。Heroku上にデプロイする前に、カスタムmodel, view, controllerを追加してもう少し味付けしてみましょう。


<hr />

*Step1) app/models/Widget.scalaを新規作成して、以下を記述します。

{% highlight sh %}
package models
 
case class Widget(id: Int, name: String)
{% endhighlight %}


<hr />

*Step2) app/views/Widget/list.scala.htmlファイルを新規作成して、以下を記述します。

{% highlight sh %}
@(widgets: Vector[models.Widget])
 
<!DOCTYPE html>
<html>
    <body>
        @widgets.map { widget => 
            Widget @widget.id = @widget.name</br>
        }
    </body>
</html>
{% endhighlight %}

<hr />

*Step3) app/controllers/WidgetController.scalaを新規作成して、以下を記述します。

{% highlight sh %}
package controllers
 
import play._
import play.mvc._
 
object WidgetController extends Controller {
 
    import views.Widget._
    import models.Widget
 
    def list = {
        val widget1 = Widget(1, "The first Widget")
        val widget2 = Widget(2, "A really special Widget")
        val widget3 = Widget(3, "Just another Widget")
        html.list(Vector(widget1, widget2, widget3))
    }
 
}
{% endhighlight %}

*Step4) 以下のURLを表示して、動きを確認しましょう。

{% highlight sh %}
http://localhost:9000/WidgetController/list
{% endhighlight %}


動きましたね。それにサーバーのリロードも不要でした。URLを少しきれいにしましょう。conf/routeファイルを修正して、”Application.index”を”WidgetController.list”へ変更します。

{% highlight sh %}
GET     /                                       WidgetController.list
{% endhighlight %}


簡単にできましたが、ついでに友達にも見せたいですね。Herokuへデプロイしましょう。

<hr />

*Step1) Linux, MacもしくはWindowsに、Herokuコマンドクライアントをインストールします。


*Step2) コマンドラインからHerokuへログインします。

{% highlight sh %}
heroku auth:login
{% endhighlight %}


*Step3) Herokuは、アプリケーションのデプロイにgitを使うので、.gitignoreファイルを作成して、以下を記述します。

{% highlight sh %}
/modules
/tmp
{% endhighlight %}


*Step4) gitリポジトリを作成して、ファイルを追加し、コミットします。

{% highlight sh %}
git init
git add .
git commit -m init
{% endhighlight %}


*Step5) Herokuにアプリケーションを新規作成します。

{% highlight sh %}
heroku create -s cedar
{% endhighlight %}


これで、Heroku上に新しいアプリケーションを規定し、アプリケーションへのランダムな名前/のURLを割り当てます。


*Step6) アプリケーションをデプロイします。

{% highlight sh %}
git push heroku master
{% endhighlight %}


アプリケーションがアセンブルされ、Heroku上にデプロイされます。


*Step7) ブラウザでアプリケーションを開きます。

{% highlight sh %}
heroku open
{% endhighlight %}


ジャジャーン！これでPlay + Scala アプリがクラウド上で実行されました。


JavaOneで、これをBill Venners(ScalaTestの作者)に見せたら、scalatest.orgのWebサイト（Play + Scalaのアプリ)をHerokuへ移行してくれました。クール！


何か質問あれば、教えてください。



