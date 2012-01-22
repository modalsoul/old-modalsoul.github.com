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
$ "C:\Proguram files(x86)\ruby-1.8\bin"
{% endhighlight %}
###RubyGemsのセットアップ

###再：Rubyをインストール
Rubyのパスを↓に変更する。
{% highlight sh %}
$ C:\ruby-1.8
{% endhighlight %}

###Jekyllのインストール
{% highlight sh %}
$ gem install jekyll
{% endhighlight %}
で、インストールできるはずなんだけど、なんかエラーがでる。。
どうやらWindows環境だと、MAKE無くてエラーになってるらしい。
Mac、Linux環境だとmakeはデフォルトはいっているので、Windows環境の嵌りポイント。

###NMAKEのインストール
こちらを参考にWindows環境用のMAKE：NMAKEのインストールを試みる。
[http://d.hatena.ne.jp/perlcodesample/20081025/1225035398](http://d.hatena.ne.jp/perlcodesample/20081025/1225035398)
![http://d.hatena.ne.jp/perlcodesample/20081025/1225035398](http://capture.heartrails.com/free?http://d.hatena.ne.jp/perlcodesample/20081025/1225035398)

Nake15.exeをダウンロード＆インストールしようとしたら、
![nmake error diag](/img/nmake15-error.jpg)

手持ちの環境では、このNMAKEは使えないらしい。
別の方法を探す。


###VisualStudioのインストール
VC++ CompilerにNMAKEが同梱されているようなので、VisualStudioをインストール。

![VisualStudio 2010 Express](http://capture.heartrails.com/free?http://www.microsoft.com/japan/msdn/vstudio/express/)

Microsoft Visual C++ 2010 Expressをインストール。
インストール後に、再度Jekyllインストールしてみてもやっぱりダメ。

{% highlight sh %}
NMAKE : fatal error U1077: '"～\bin\cl.EXE"' : リターン コード '0xc0000135'
{% endhighlight %}

まだ何か足りないっぽい。

###環境変数の設定

環境変数を設定してやる必要があるらしい。

![http://www7.atwiki.jp/smashonline/?cmd=word&word=cl.exe%200xc0000135&type=normal&page=nmake](http://capture.heartrails.com/free?http://www7.atwiki.jp/smashonline/?cmd=word&amp;word=cl.exe%200xc0000135&amp;type=normal&amp;page=nmake)

{% highlight sh %}
$ C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\bin\vcvars32.bat
{% endhighlight %}

###再：Jekyllのインストール

{% highlight sh %}
$ gem install jekyll
{% endhighlight %}

なんとかJekyllのインストールに成功。


##Jekyllの実行

ローカルのgithub:pagesのルートディレクトリ配下に移動してJekyllを実行。

{% highlight sh %}
$ jekyll --server --auto
{% endhighlight %}

##ブラウザで表示

デフォルトのポートが4000なので、

http://localhost:4000

にアクセスする。

##最後に

時間がたってからこの記事を書いたので、一部詳細があってないところもあるだろうけれど
大体こんな流れでした。
