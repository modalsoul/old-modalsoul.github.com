---
layout: post
title: 横浜JSTDDハンズオンに参加しました。#JSTDD
tags: TDD JsTestDriver JavaScript
categories: report Test
---
JavaScriptテスト駆動開発ハンズオン
-----------------

![横浜JSTDDハンズオン：ATND](http://capture.heartrails.com/300x200/cool?http://atnd.org/events/25519)

横浜JSTDDハンズオンに参加してきました。
概要は、JavaScriptのテスト駆動開発についてのあれこれについてで、

会場の[「タネマキ」](http://tane-maki.net/)は、なんかワクワクするカッコいいところでした！
![コワーキングスペースの「タネマキ」](https://yukar.in/p/pz/123506436)

当日のつぶやきまとめは[こちら](https://yukar.in/note/ckFoT5)

[横浜JSTDDハンズオン #JSTDD つぶやきまとめ - Yukarin'Note](https://yukar.in/note/ckFoT5)


[@kyo_ago さん発表資料](http://0-9.tumblr.com/post/17020645713/jstestdriver-asynctestcase)

[@kyo_ago さん補足資料](https://yukar.in/note/ckFp4b)

[@azu_re さん発表資料](http://efcl.info/2012/0226/res3015/)

[@os0x さん発表資料]()

<hr />
##@kyo_agoさん発表
![ハンズオン風景](https://twitpic.com/show/large/8osh18)
###TDDって？
まずは簡単なアンケート

今日のハンズオンが初TDDの人？
<blockquote class="twitter-tweet" lang="ja"><p>初TDD率は、5/20人くらい？ <a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a></p>&mdash; imae （Master BBQ）さん (@modal_soul) <a href="https://twitter.com/modal_soul/status/173620307347783680" data-datetime="2012-02-26T04:08:01+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>

JavaScriptのテストをしたことがある人？
※初TDDの人を含まず。
<blockquote class="twitter-tweet" lang="ja"><p>JavaScriptのテスト　3/15人くらい？ <a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a></p>&mdash; imae （Master BBQ）さん (@modal_soul) <a href="https://twitter.com/modal_soul/status/173620451766050816" data-datetime="2012-02-26T04:08:36+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>

JsTestDriverを使ったことがある人？
<blockquote class="twitter-tweet" lang="ja"><p>JsTestDriverやったことある率は、3/20人くらい？ <a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a></p>&mdash; imae （Master BBQ）さん (@modal_soul) <a href="https://twitter.com/modal_soul/status/173620584972943360" data-datetime="2012-02-26T04:09:07+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>


そもそもテストって？
<blockquote class="twitter-tweet" lang="ja"><p>「プログラマがおこなうホワイトボックステスト」って、１．開発的アプローチ（TDD）、2.ドキュメント的アプローチ、3.品質担保的アプローチ <a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a></p>&mdash; imae （Master BBQ）さん (@modal_soul) <a href="https://twitter.com/modal_soul/status/173622536502906880" data-datetime="2012-02-26T04:16:53+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>


###JsTestDriverについて
![JsTestDriverの説明](http://gyazo.com/afad732dae49b26e0e44f1897d67b010.png)

[JsTestDriver](http://code.google.com/p/js-test-driver/)についての説明と、テストをハンズオン形式で実施。

ハンズオンの手順、設定ファイル、プロダクトコード、テストコードは、

[こちら](https://yukar.in/note/ckFp4b)に沿ってやりました。

テストコードの書き方、流れ、設定のTips、loggingなどなど

なかでもドキュメントやヘルプにもでてこない[オプション](http://code.google.com/p/js-test-driver/source/browse/trunk/JsTestDriver/src/com/google/jstestdriver/config/YamlParser.java#96)がでてきて新鮮でしたｗ

<blockquote class="twitter-tweet" lang="ja"><p>これは「変なことをやりたかったらJSTestDriverのコードを読もうぜ」フラグ…… <a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a></p>&mdash; さねゆきさん (@saneyuki_s) <a href="https://twitter.com/saneyuki_s/status/173642014301224960" data-datetime="2012-02-26T05:34:16+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>
Google ProjectのWikiを読むよりも[コード](http://code.google.com/p/js-test-driver/source/browse/trunk/JsTestDriver/src/com/google/jstestdriver/config/YamlParser.java#96)を見たほうが色々わかるよ、って

###Sinon.jsについて
JsTestDriverで非同期処理のテストを使用とすると、
（かなり）難しいことになるようなので、

<blockquote class="twitter-tweet" lang="ja"><p>jsTestDriverで非同期テストの設定を書くのはあまりに面倒なので向いてない(かも) <a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a></p>&mdash; gochoさん (@gocho) <a href="https://twitter.com/gocho/status/173639264653942786" data-datetime="2012-02-26T05:23:21+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>
Sinon.jsでやったほうがいいところも（多々）あるそうです。

この辺りは適材適所で使い分けるのがポイントのようです。
<blockquote class="twitter-tweet" lang="ja"><p>ajaxのテストは、jsTestDriverだけじゃなくて、Sinon.jsとか、jqMockとか、使うといいんじゃない? 適材適所大切。 <a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a></p>&mdash; gochoさん (@gocho) <a href="https://twitter.com/gocho/status/173642294325555202" data-datetime="2012-02-26T05:35:23+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>

##@azu_reさん発表
###WebStormについて
私は使ったこと無かったのですが、シェアウェアのIDEなんだそうです。

<blockquote class="twitter-tweet" lang="ja"><p><a href="https://twitter.com/search/%2523JSTDD">#JSTDD</a> 発表中のスライド "WebStorm指南書" <a href="http://t.co/t72fnvYh" title="http://bit.ly/wbPl1J">bit.ly/wbPl1J</a></p>&mdash; azuさん (@azu_re) <a href="https://twitter.com/azu_re/status/173658374586646528" data-datetime="2012-02-26T06:39:17+00:00">2月 26, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>

有償なだけあり、豊富な機能が提供されていてとても便利そうな印象でした。

@azu_reさんの資料がとても良くまとめられていらっしゃるので、詳細は[こちら](http://azu.github.com/slide/webstorm/webstorm.html)を見てもらったほうがいいと思います。
![WebStorm指南書](http://capture.heartrails.com/200x150/cool?http://azu.github.com/slide/webstorm/webstorm.html)

##@os0xさん発表
###jasmine-headless-webkitについて
![jasmine-headless-webkit -- The fastest way to run your Jasmine specs!](http://capture.heartrails.com/200x150/cool?http://johnbintz.github.com/jasmine-headless-webkit/)

JasmineのspecをCUI環境でいい感じにテストできるツールなんだそうです

[参考](https://github.com/pivotal/jasmine/wiki/Asynchronous-specs)

　#JSTDDのTL上に特に資料のURLがなかったので、この記事ではここまでしかかけてません、すみません


##まとめ
全体を通して、なかなかコアな話が聞けてとてもためになりました。

[テスト駆動JavaScript](http://www.amazon.co.jp/gp/product/4048707868/ref=as_li_ss_tl?ie=UTF8&tag=modalsoul-22&linkCode=as2&camp=247&creative=7399&creativeASIN=4048707868)を去年読みはじめてから、
もっと勉強しよう、と思いつつなかなか手がつけられずにいたので、
とてもいい機会になりました。

<a href="http://www.amazon.co.jp/gp/product/4048707868/ref=as_li_ss_il?ie=UTF8&tag=modalsoul-22&linkCode=as2&camp=247&creative=7399&creativeASIN=4048707868"><img border="0" src="http://ws.assoc-amazon.jp/widgets/q?_encoding=UTF8&Format=_SL110_&ASIN=4048707868&MarketPlace=JP&ID=AsinImage&WS=1&tag=modalsoul-22&ServiceVersion=20070822" ></a><img src="http://www.assoc-amazon.jp/e/ir?t=modalsoul-22&l=as2&o=9&a=4048707868" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


今回の内容を踏まえつつ、

Jenkinsとからめてビルドパイプラインを構築するのが当面の目標ですね。

