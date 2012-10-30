---
layout: post
title: node.js+JSDoc3によるインラインAPIドキュメントの生成
tags: node.js JavaScript JSDoc3
categories: report
comment: この記事は、node.js+JSDoc3によるインラインAPIドキュメントの生成についてのメモ書きです。
thumbnail: 
---
<a href="http://www.flickr.com/photos/pixagraphic/3414730395/" title="Old Documents by pixagraphic, on Flickr">
<img src="http://farm4.staticflickr.com/3586/3414730395_7dedac534b_n.jpg" width="320" height="213" alt="Old Documents">
</a>
-----------------
## 謝辞
以下の記事を参考にさせていただきました。感謝です。


*・ [JsDoc3-manual-jp](https://sites.google.com/site/jsdoc3manualjp/)
*・ [JavaScript対応APIドキュメント生成ツールのまとめ | Web scratch](http://efcl.info/2011/0226/res2291/)

<hr />

## JavaScriptのドキュメンテーション

今回は「JavaScriptでコメントを書くときどうすればいいんだ？」という疑問から出発しました。
Javaだと、JavaDocの形式で書いておけば、Eclipse等のIDEを使うと対象のメソッドをわざわざ見に行かなくともコメント、引数、戻り値が見れて便利です。

またJavaScriptの場合、静的型付け言語のように型システムによる補助が受けられないので、個人的にものぐさからJavaScriptでいい感じにコメント・ドキュメンテーションが残せるものを調べました。

<hr />

## JSDoc3を使う

ドキュメント生成ルーツは色々あるようですが、今回はJSDoc3を使いました。
<a href="https://github.com/jsdoc3/jsdoc">
<img title="jsdoc3/jsdoc ・ GitHub" src="http://capture.heartrails.com/120x90/cool/1351633993677?https://github.com/jsdoc3/jsdoc" alt="https://github.com/jsdoc3/jsdoc" width="120" height="90" />
</a>

現状だとJSDocの後継"JsDoc Toolkit"がメジャーなようですが、さらに後継のJSDoc3を使いました。

## JSDoc3のインストール
JSDoc3をインストールします。
npmが既にインストールされている前提で

{% highlight sh %}
npm install -g git://github.com/jsdoc3/jsdoc.git
{% endhighlight %}

npmはとても便利ですね


## アノテーションの記述

ドキュメントに落としたいオブジェクトの直前にJSDocアノテーションを追加します。
Javaと同じく"/** ... */ "で囲まれた部分がアノテーションとなります。

タグは、"@... "で記述します。

実際にnode.js+expressのコードにアノテーションを追加してみました。

<a href="https://github.com/modalsoul/expressSample/blob/master/app.js">
<img title="expressSample/app.js at master ・ modalsoul/expressSample ・ GitHub" src="http://capture.heartrails.com/200x150/cool/1351634631326?https://github.com/modalsoul/expressSample/blob/master/app.js" alt="https://github.com/modalsoul/expressSample/blob/master/app.js" width="200" height="150" />
</a>


## タグの種類
上記のサンプルでは@param, @return, @moduleの3種類しか使っていませんが、JSDocには他にもタグが用意されているようです。

### 主要属性

 @param  @return  @extends 

### アクセス識別子

 @private  @protected  @public  @access 

### オブジェクト属性

 @class  @constructor  @function  @constant  @member  @enum  ...

### オブジェクト参照属性

 @static  @inner 

### 説明

 @desc  @example  @fileoverview  @classdesc ...

### サポート情報

 @version  @since  @deprecated  @see  @author 
 
 
### ドキュメントの生成
JSDocを実行して、ドキュメントを生成します。

{% highlight sh %}
jsdoc app.js
{% endhighlight %}

[生成されたドキュメントはこちら](http://modalsoul.github.com/assets/sample/jsdoc-out/56ea328d1a.html)

