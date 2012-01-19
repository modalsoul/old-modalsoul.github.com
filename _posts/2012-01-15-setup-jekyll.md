---
layout: post
title: Jekyllによる静的Ｗebサイト構築入門してみた
tags: Jekyll
categories: record
---
[Jekyllによる静的Ｗebサイト構築入門してみた](http://www.ksr-it.net/pdf/kushiro-jekyll-text.pdf)
-----------------

![Jekyllによる静的Ｗebサイト構築入門](http://capture.heartrails.com/free?http://www.ksr-it.net/pdf/kushiro-jekyll-text.pdf)

[こちら](http://www.ksr-it.net/pdf/kushiro-jekyll-text.pdf)を参考にしました。


いろいろ嵌りポイントがあったので、後世の遺産として書き残します。

##環境構築
※ここで使った環境は、Windows7です。
###Rubyのインストール
ActiveScriptRubyを[ダウンロード](http://www.geocities.co.jp/SiliconValley-PaloAlto/9251/ruby/)＆インストール。
デフォルト設定でインストールしたら、↓に入った。(うろ覚え、たしかあってると思う)
{% highlight sh %}
$ C:\Proguram files(x86)\ruby-1.8
{% endhighlight %}

###Pathの設定
環境変数に↓を追加
{% highlight sh %}
C:\Proguram files(x86)\ruby-1.8\bin
{% endhighlight %}
###RubyGemsのセットアップ

###再：Rubyをインストール
Rubyのパスを↓に変更する。
{% highlight sh %}
C:\ruby-1.8
{% endhighlight %}

###
###Jekyllのインストール
