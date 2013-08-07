---
layout: post
title: node.jsをはじめました
tags: node.js 入門
categories: report
comment: この記事は、node.js使い始めのメモ書きです。
thumbnail: http://nodejs.jp/index.html
---

-----------------


## 謝辞
今回、node.jsをはじめるに辺りこちらのサイトを参考にさせていただきました。感謝です。

![node.js ハンズオン資料](http://capture.heartrails.com/200x150/cool?http://dl.dropbox.com/u/219436/node.js/handson/build/html/index.html)

<hr />

## インストール

まずは、環境の準備です。

上記ハンズオン資料にもUnixシステムへのインストール手順が記載されていますが、手持ちの環境がWindowsなので若干異なりました。


*1. インストーラのダウンロード*

下記サイトから最新の安定版をダウンロード。2012/10/10時点の最新の安定版は、v0.8.11

![v0.8のインストーラダウンロード](http://capture.heartrails.com/200x150/cool?http://nodejs.jp/nodejs.org_ja/docs/v0.8/)

*2. インストーラの起動*

画面に従いインストールを完了します。

*3. インストールの確認*

コマンドラインからインストールしたnode.jsのバージョンを確認します。v0.8.11の表示が確認できます。

{% highlight sh %}
$ node -v
v0.8.11
{% endhighlight %}

*4. npmのインストール*
npmは、nodeのライブラリパッケージ専用のパッケージ管理ツールです。
インストーラからインストールすると、併せてnpmもインストールされます。


## Hello World
helloworld.jsを作成します。
<script src="https://gist.github.com/3868393.js"> 
</script>

実行すると、こんな感じ。

{% highlight sh %}
$ node helloworld.js
Hello node.js
{% endhighlight %}


## 非同期IOメソッドを使う
ハンズオンの内容を写経してみます。

### 1ファイルのダウンロード

<script src="https://gist.github.com/3868441.js"> 
</script>

これを実行すると、こんな感じ。

{% highlight sh %}
$ node download1.js http://www.google.co.jp/
http.createClient is deprecated. Use `http.request` instead.
200
date:Wed, 10 Oct 2012 21:17:40 GMT
expires:-1
cache-control:private, max-age=0
content-type:text/html; charset=Shift_JIS
set-cookie:PREF=ID=e0429b2fdb66b6f5:FF=0:TM=1349903860:LM=1349903860:S=RztyFRMdq8JPKHw1; expires=Fri, 10-Oct-2014 21:17:40 GMT; path=/; domain=.google.co.jp,NID=64=bF0HOrYnQ_hHT1a9O-_qkflB9ZOIv34SmPnV4YeXbEbhqCQGR0wjpKvjFPtnQ0psZzJQPW3qZvP7uaHjxg_ajr0--fMSSVCFvOzHFe_O6Eqc2wCs-VZqV-7L3-1PnQ9V; expires=Thu, 11-Apr-2013 21:17:40 GMT; path=/; domain=.google.co.jp; HttpOnly
p3p:CP="This is not a P3P policy! See http://www.google.com/support/accounts/bin/answer.py?hl=en&answer=151657 for more info."
server:gws
x-xss-protection:1; mode=block
x-frame-options:SAMEORIGIN
transfer-encoding:chunked

・・・
{% endhighlight %}


実行結果の1行目に何やらメッセージが出力されていますが、リファレンスで確認するとhttp.createClientの使用は非推奨とのことでした。

![http.createClientは非推奨らしい](http://capture.heartrails.com/200x150/cool?http://nodejs.jp/nodejs.org_ja/docs/v0.8/api/http.html#http_http_createclient_port_host)


http.request()で書き直したものがこちら。

<script src="https://gist.github.com/3868625.js"> 
</script>

同じように動きます。
