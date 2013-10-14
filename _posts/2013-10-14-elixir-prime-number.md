---
layout: post
title: elixirで素数判定
tags: elixir 素数
categories: programing
comment: この記事は、社内のプログラミングコンテストで、Scalaの素数判定のプラグラムを書いたついでにelixirで同じプログラムを書いた時のメモ書きです。
thumbnail: 
---

![素数](http://kajipon.sakura.ne.jp/art/j-olym45.jpg)

## 概要
[前回Scalaで実装したもの](/programing/2013/10/09/scala-prime-number/)と同じく、1<= X <=10^9 の範囲の自然数Xを渡されたとき、Xが素数か否かを判定します。

こちらも同様に判定には、試し割り法を使いました。

<hr/>
## コード
↓のようになりました

<script src="https://gist.github.com/modalsoul/6970624.js">
</script>

<hr/>
## 解説
def isPrimeがメイン関数です。

* 与えられた数値numが、1 or 偶数ならfalseを返す。
* ↑以外の場合、3から数値numの平方根までのリストを生成して、sieveをコールする。


前回Scalaでは試し割りをループで実行しましたが、elixirでは再帰で実行しています。

というか、elixirではErlangと同じく条件ループがもとからありません。

同じようにリストの生成も、Scalaでは (x to y)の形式で行えましたが、elixirでは def makeListを再帰して生成しています。
Scalaと同じようにリストを生成するような便利な関数があるのかもしれませんが、見つけられなかったので。。


def seiveのコールでは、第１引数のリストが空でない場合、def seive([h｜t], num)、第１引数のリストが空の場合、def seive([], _)にマッチします。

def seive([h｜t], num) では、

* リストの先頭の値hで数値numで割り切れる場合、素数ではないのでfalseを返し、
* 割り切れない場合、先頭以外のリストtと数値numを再度seiveに渡します。

最終的に、リスト末尾まで試し割りし、割り切れなかった場合、def seive([], _)にマッチし、trueが帰ります。


Scalaで書いたものと比較して、関数が増えていますが、中の処理自体はよりさっぱりした感じになりました。
まだelixir慣れしていないので、もっと良い書き方ができるのだろうとは思いますが、今回はこんなところで。
