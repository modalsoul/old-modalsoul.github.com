---
layout: post
title: node.jsにおける、npmとpackage.jsonという作法について
tags: node.js npm package.json
categories: report
comment: この記事は、node.jsを使ったプロジェクトを作成するときの作法についてのメモ書きです。
thumbnail: https://npmjs.org/
---

-----------------
## 謝辞
以下の記事を参考にさせていただきました。感謝です。


*・ [npm についてまとめてみる - need something more...](http://kysnm.hatenablog.com/entry/20111225/1324828008)


*・ [npm configで設定値を管理 - すぎゃーんメモ](http://d.hatena.ne.jp/sugyan/20110909/1315575343)


*・ [Node.js の起動オプション、環境変数、npm start の話 - Block Rockin’ Codes](http://d.hatena.ne.jp/Jxck/20120410/1334071898)


## npmとpackage.jsonによるパッケージ管理

node.jsではパッケージ管理にnpmを用います。そしてRubyのGemfileのように、パッケージのバージョン管理をするには、package.jsonファイルを用います。

<script src="https://gist.github.com/3914746.js"> 
</script>
こんな感じ。

上記のようにバージョンを直接指定する以外にも、"xx以上"といった指定もできるようです。
{% highlight sh %}
"express": ">=2.3.11",
{% endhighlight %}


package.jsonの編集が終わったら、
{% highlight sh %}
npm install
{% endhighlight %}
これで、一括でパッケージがインストールされます。

パッケージは、./node_modules配下にインストールされるので、git等でバージョン管理を行う場合、.gitignoreにnode_modules/を追加すると良いようです。


<hr />

## npm startによる起動とpackage.jsonでの設定

package.jsonには、パッケージの依存関係以外にも色々記述できます。
例えばscriptsに、prestart,start,poststartを記述しておくと、npm startで実行されるようです。

以下のような、package.jsonを記述した場合、
{% highlight sh %}
"scripts": {
    "start": "NODE_ENV=production NODE_PATH=lib node app"
{% endhighlight %}

npm installを実行することで、実行環境を切り替え、モジュールをPATHに追加し、起動します。

<hr />
## まとめ
プロジェクトを作成する人は、package.jsonをキチンと書く。


cloneして使う人は、npm installして、npm startする。

