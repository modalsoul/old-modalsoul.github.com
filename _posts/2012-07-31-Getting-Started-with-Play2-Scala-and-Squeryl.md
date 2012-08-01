---
layout: post
title: 【意訳】Scala on Play2 with Squerylではじめるデータベース駆動アプリ　#play_ja
tags: Play2 Heroku Database-Driven Scala Squeryl CoffeScript JSON jQuery ScalaTest
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
データベースインテグレーションの最善の技術はこれまで重ねて討論されてきました。

