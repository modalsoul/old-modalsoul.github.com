---
layout: post
title: 【意訳】データベース駆動アプリのための、Play2最適化について 
tags: Play2 Heroku Database-Driven
categories: Programing
---
【意訳】データベース駆動アプリのための、Play2最適化について  
-----------------

この記事は、HerokuのPrincipal Developer Evangelistの[James Ward氏の記事](http://www.jamesward.com/2012/06/25/optimizing-play-2-for-database-driven-apps)の意訳です。参考程度にどうぞ。

<hr />

先週Matt Raible氏と共に、UberConfで行われたPlay vs. Grailsスマックダウンでプレゼンテーションをしました。
このセッションの目標は、同じフレームワークを用いて同じアプリケーションを作り、Play2+JavaとGrailsを比較することです。
比較のために、いくつかのベンチマークを含む評価基準を使用しました。
結果については、こちら（[Matt's session recap blog](http://raibledesigns.com/rd/entry/play_vs_grails_smackdown_at)）に詳しく掲載されています。
結果プレゼンの後、これらのタイプのアプリケーションでPlay2のAkkaスレッドシステムを最適化するのに重要なことを学びました。
Play2は、ブロッキングのコールを含まないHTTPリクエスト(i.e. 非同期)のためにout-of-the-boxに最適化されています。
Javaのほとんどのデータベース駆動アプリでは同期コールはJDBC経由で使用されるため、Play2ではこれらのリクエストタイプのためにAkkaを調整するちょっとした設定が必要です。

シンプルな例をあげてみます。時間的に現実的なデータベースブロックをシミュレートするために、このベンチマークをローカルではなくHeroku上で実行します。
["play2bars"example app](https://github.com/jamesward/play2bars)から"java-ebean"ブランチとHerokuの共有のPostgreSQLデータベースを使います。

Apache Benchを実行して、同時100クライアント接続で10000リクエストをアプリのJSONサービスへ投げるために、同じAWSリージョンにあるEC2サーバーからHerokuのmy appとして、以下のコマンドを実行しました。



最初の最適化前の結果がこちらです

秒間973リクエストは悪くはありませんが、下記のようなエラーが確認できました。



これは、Akkaが長時間ブロックされたことを意味しており、Play中のAkkaの設定を変え、同期のコードハンドリングを改善して、評価を再実行しました。







