---
layout: post
title:【意訳】Scala　on　Play2　with　Squerylではじめるデータベース駆動アプリ　#play_ja
tags:Play2 Heroku Database-Driven Scala Squeryl CoffeScript JSON jQuery ScalaTest
categories: Programing
---
【意訳】Scala on Play2 with Squerylではじめるデータベース駆動アプリ
-----------------

この記事は、HerokuのPrincipal Developer EvangelistのJames Ward氏とRyan Knight氏の記事の意訳です。参考程度にどうぞ。

![Getting Started with Play 2, Scala, and Squeryl](http://capture.heartrails.com/300x200/cool?http://www.artima.com/articles/play2_scala_squeryl.html)

誤訳・誤植等ありましたら、[@modal_soul](https://twitter.com/modal_soul)までリプライいただけるとありがたいです。
<hr />

## サマリー
<table><tr><td>
この記事では、Play2,Scala,Squeryl,JSON,CoffeScript,CoffeScript,jQueryとScalaTestを使ったWebアプリケーションの作り方を紹介します。また、同時にScalaTestを使ったテスト方法とHerokuを使ったクラウド環境へのアプリケーションデプロイについても学べます。
</td></tr>
</table>


Play2,Scala,Squeryl,JSON,CoffeScript,jQueryとScalaTestの組み合わせによって、モダンなWebアプリケーションの構築とテストを効率的に行うことができます。
シンプルなデータベース駆動Webアプリケーションの構築、テスト、クラウドへのデプロイを
