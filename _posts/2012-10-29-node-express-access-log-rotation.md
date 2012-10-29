---
layout: post
title: node.js+express+log4jsでのアクセスログローテーション
tags: node.js express log4js
categories: report
comment: この記事は、node.jsとexpressとlog4jsを使ったアクセスログのファイル出力とローテーションについてのメモ書きです。
thumbnail: 
---
<a href="http://www.flickr.com/photos/kuromatsunai/3024100752/" title="足跡 - Footprint - by kuromatsunai.info, on Flickr">
<img src="http://farm4.staticflickr.com/3272/3024100752_36c698bab8_n.jpg" width="320" height="240" alt="足跡 - Footprint -">
</a>
-----------------
## 謝辞
以下の記事を参考にさせていただきました。感謝です。


*・ [【Node.js】expressでaccess log + log rotate | 湘南社中テクニカルブログ](http://blog.shonanshachu.com/2012/10/nodejsexpressaccess-log-log-rotate.html)

<hr />

## node.jsとexpressでのアクセスログ

node.js+expressには、express標準のloggerだとログの出力先をファイルにすると、対象のファイルがずっとオープン状態になってしまう、という欠点があります。
(※:もしかしたら私が回避方法を知らないだけかもしれませんが。）

そのため、いい感じのログをファイル出力し、日次でローテーションする、というような運用ができません。

1ファイルに長々とアクセスログが出力されるようでは、運用もままならないので、log4jsを使ってexpressのアクセスログをいい感じにローテーションできるようにしたいと思います。

<hr />

## log4jsを使う

LoggingFrameworkのlog4jsについては、こちらで紹介しています

[node.js+log4jsで、ログローテーションする](http://modalsoul.github.com/report/2012/10/14/node-js-log4js/)

<a href="http://modalsoul.github.com/report/2012/10/14/node-js-log4js/">
<img title="Now Capturing..." src="http://capture.heartrails.com/120x90/cool?http://modalsoul.github.com/report/2012/10/14/node-js-log4js/" alt="http://modalsoul.github.com/report/2012/10/14/node-js-log4js/" width="120" height="90" />
</a>


実際のコードはこんな感じ。※:要リファクタリング
<script src="https://gist.github.com/3973839.js">
</script>
app.routerの前で、functionを入れログレベルをinfoで出力させています。



実際にexpressをスタートさせ、ブラウザからアクセスすると以下のようなログが出力されます。

access.log
{% highlight sh %}
[2012-10-29 23:06:18.732] [INFO] dateFile - 127.0.0.1	Mon Oct 29 2012 23:06:18 GMT+0900 (東京 (標準時))	GET	/	200	-	Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.4 (KHTML, like Gecko) Chrome/22.0.1229.94 Safari/537.4
[2012-10-29 23:06:18.968] [INFO] dateFile - 127.0.0.1	Mon Oct 29 2012 23:06:18 GMT+0900 (東京 (標準時))	GET	/stylesheets/style.css	200	http://localhost:3000/	Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.4 (KHTML, like Gecko) Chrome/22.0.1229.94 Safari/537.4
{% endhighlight %}



