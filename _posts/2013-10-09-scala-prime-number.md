---
layout: post
title: Scalaで素数判定【追記アリ】
tags: Scala 素数
categories: programing
comment: この記事は、社内のプログラミングコンテストで、Scalaの素数判定のプラグラムを書いた時のメモ書きです。
thumbnail: 
---

![素数](http://kajipon.sakura.ne.jp/art/j-olym45.jpg)

## 概要
今回は、1<= X <=10^9 の範囲の自然数Xを渡されたとき、Xが素数か否かを判定します。

判定には、試し割り法を使いました。

<hr/>
## コード
↓のようになりました

<script src="https://gist.github.com/modalsoul/6902340.js">
</script>

<hr/>
## 解説
処理のおおまかな流れは以下のような感じ

* 対象の数字Xを受け取る
* Xが１か偶数だったら、素数ではない
* 3からXの平方根までのリストを作る
* リストから偶数を除去
* リストの先頭から順に、Xがその数で割り切れるかどうかを調べる
* 割り切れたら、素数ではない
* 割り切れずに、リストの末尾まで到達したら、素数	
* 結果を表示


(x to y).filter().foreach() の形式で書きたかったのでこんな設計になりました。
なんとなくScalaっぽくてこの書き方が好きなので

foreachの中の処理で割り切れた場合、明示的なreturnでループを抜けています

対象の数が大きくなると生成されるリストも大きくなり、ループが長くなるので
filterの条件を増やして(ex. 3, 5, 7, 11,,,)リストが短くなるようにしてみたりもしたんですが、
早くなりませんでした、というか若干遅くなった。。。

filterを追加することでループの回数を減らすことの損益分岐点は、思ったより低かったようです。無念

<hr/>
## 追記
[@gakuzzzz](https://twitter.com/gakuzzzz)さんにコメントいただき、コードを改良しました。

<blockquote class="twitter-tweet">
    <p>
        <a href="https://twitter.com/modal_soul">@modal_soul</a> (3L to sqrt(num).toLong by 2) とかすると filter が必要なくなって速くなりそうですね。
    </p>&mdash; がくぞ (@gakuzzzz) 
    <a href="https://twitter.com/gakuzzzz/statuses/387965601525669888">October 9, 2013</a>
</blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>

### コード・その２
<script src="https://gist.github.com/modalsoul/6918594.js">
</script>

(x to y by 2)とすることで、2刻みでリストを生成するので、最初に生成されるリストが短くできる&filterが省略できるわけですね。

Before:140ms, After:130ms だいたい10msほど速くなりました。


### コード・その３
<script src="https://gist.github.com/modalsoul/6918797.js">
</script>

returnを使うと例外で遅くなるのでは？という話もあったので、forallを使うパターンも試してみました。
結果は↑とほぼ同じ結果になりました。
forallがbooleanを返すので、returnのためのtrue/falseがなくなったので、コードはだいぶ綺麗になった気がします。
