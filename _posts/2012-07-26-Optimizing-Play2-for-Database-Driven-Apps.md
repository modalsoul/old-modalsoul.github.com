---
layout: post
title: 【意訳】データベース駆動アプリのための、Play2最適化 
tags:Play2 Heroku Database-Driven
categories: Programing
---
【意訳】データベース駆動アプリのための、Play2最適化  
-----------------

この記事は、HerokuのPrincipal Developer Evangelistの[James Ward氏の記事](http://www.jamesward.com/2012/06/25/optimizing-play-2-for-database-driven-apps)の意訳です。参考程度にどうぞ。

<hr />

先週Matt Raible氏と共に、ÜberConfで行われたPlay vs. Grailsスマックダウンでプレゼンテーションをしました。
このセッションの目標は、同じフレームワークを用いて同じアプリケーションを作り、Play2+JavaとGrailsを比較することです。
比較のために、いくつかのベンチマークを含む評価基準を使用しました。
結果については、こちら（[Matt's session recap blog](http://raibledesigns.com/rd/entry/play_vs_grails_smackdown_at)）に詳しく掲載されています。
結果プレゼンの後、これらのタイプのアプリケーションでPlay2のAkkaスレッドシステムを最適化するのに重要なことを学びました。
Play2は、ブロッキングのコールを含まないHTTPリクエスト(i.e. 非同期)のためにout-of-the-boxに最適化されています。
Javaのほとんどのデータベース駆動アプリでは同期コールはJDBC経由で使用されるため、Play2ではこれらのリクエストタイプのためにAkkaを調整するちょっとした設定が必要です。

シンプルな例をあげてみる。現実的なデータベースブロックをシミュレートするために、


