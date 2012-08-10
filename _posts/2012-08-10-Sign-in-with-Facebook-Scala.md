---
layout: post
title: 【意訳】Scalaを使ったFacebookサインイン
tags: Scala Facebook Lift
categories: Programing
---
【意訳】Scalaを使ったFacebookサインイン
-----------------

この記事は、[ayushmishra2005氏の記事](http://blog.knoldus.com/2012/07/29/providing-a-sign-in-with-facebook-functionality-using-scala/)の意訳です。参考程度にどうぞ。

![Providing a “Sign-in with Facebook” functionality using Scala](http://capture.heartrails.com/300x200/cool?http://blog.knoldus.com/2012/07/29/providing-a-sign-in-with-facebook-functionality-using-scala/)

誤訳・誤植等ありましたら、[@modal_soulまで](https://twitter.com/modal_soul)リプライいただけるとありがたいです。

<hr />

ここ最近に、Lift2.4で構築したソーシャルプロジェクトで、Facebookへのサインイン機能を統合しました。この記事はその時の手順のサマリです。


*1)Facebook APIを作る（※既に持っている場合不要です）

次のリンクを参考にアプリを作ってください。サイトURLを含む全ての詳細を記入してエンターします。サイトURLは以下のような感じになります。

http://www.com/api/facebook/auth


このサイトURL宛に、Facebookはレスポンスを送ります。このアプリケーションを保存すると、アプリキー/APIキー/秘密鍵が入手できます。

これらのキーは後ほど使うので注意してください。


*2) Liftを使っている場合は、各キーをdefault.propsに追加設定します。

{% highlight sh %}
facebook.key=<your_key>
facebook.secret=your <secret_key>
facebook.callbackurl=/api/facebook/auth
{% endhighlight %}


*3) Facebookログイン画像[fb.png](http://sandthre34.wapka.mobi/download-43-23c50858c8a88d7a9397/fb.png?PHPSESSID=3b88fe8fb9bb628c5beb8870fd0e367c)をダウンロードします


*4) アプリキー/APIキー/秘密鍵とコールバックURLを設定するために、FacebookGraph.scalaを新規作成します。
