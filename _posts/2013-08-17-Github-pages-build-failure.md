---
layout: post
title: Github-pagesでPage　build failureにハマった件。またはLiquid　Exception：　invalid　byte　sequence　in　UTF-8になった件。
tags: Github-pages Jekyll
categories: environment
comment: この記事は、Github-pagesでビルドに失敗する現象にハマった時のメモ書きです。
thumbnail: http://jekyllrb.com/
---

## Githubからの通知の確認
Github-pagesのリポジトリへpushすると以下のような文面のメールが届きます。

	Subject: Page build failure

	The page build failed with the following error:

	page build failed

	For information on troubleshooting Jekyll see https://help.github.com/articles/using-jekyll-with-pages#troubleshooting
	If you have any questions please contact GitHub Support.

	support@github.com
	https://github.com/contact

通知してくれるのはいいんですが、情報量が少ない。。せめてビルド時のエラーログなりを送ってほしい。。。


## Githubのヘルプページの確認
ひとまず、メールに記載されていたURLのページを確認します。

![Troubleshooting｜Using Jekyll with Pages｜GitHub Help](http://capture.heartrails.com/128x128?https://help.github.com/articles/using-jekyll-with-pages#troubleshooting)

Troubleshootingに記載されている要旨は、「ローカルでビルドして、パースエラーを見つけなさい」とのこと。


### ローカル環境でビルド
GitHubで使っているJekyllとその依存関係のバージョンについては、[the same versions of Jekyll and other dependencies that we use](https://github.com/github/pages-gem/blob/master/github-pages.gemspec#L16)に記載されていて、この時点では以下のバージョンになっていた。

	  s.rubygems_version      = "1.8.0"
	  s.required_ruby_version = "~> 1.9.3"
	  ~~~
	  # Jekyll and related dependency versions as used by GitHub Pages. 
	  # For more information see:
	  # https://help.github.com/articles/using-jekyll-with-pages

	  s.add_dependency("RedCloth",   "= 4.2.9")
	  s.add_dependency("jekyll",     "= 1.1.2")
	  s.add_dependency("kramdown",   "= 1.0.2")
	  s.add_dependency("liquid",     "= 2.5.1")
	  s.add_dependency("maruku",     "= 0.6.1")
	  s.add_dependency("rdiscount",  "= 1.6.8")
	  s.add_dependency("redcarpet",  "= 2.2.2")


ローカルの環境を上記のバージョンに合わせてビルドしてみると、いくつかErrorが。

エラーの原因は、
* _postの記事に書かれていたサンプルコードがmarkdownのリンク表記とかぶっていたため、不正なリンクとして解釈されていた
* URLにマルチバイト文字を含むためエラーになった
こと


上記の箇所を修正してローカル環境でビルドしてもエラーや警告が出ないことを確認し、GitHubに再度pushしても、同じくPage build failureとなってしまった。

ひとまずバージョン依存での問題ではなさそうということはわかった。


## Pages don't build: "Unable to run Jekyll"の確認
GitHubのヘルプには、Troubleshootingの手順で確認してもビルドに失敗したら、[Page doesn't build](https://help.github.com/articles/pages-don-t-build-unable-to-run-jekyll) guideを確認するようにと記載されていた。


このPage does'nt build guideの内容は以下

### Unsafe plugins
危ないプラグインは使うなよ、ということらしいけど、debug.rbしか入っていないから、これが原因ではなさそう。


### Syntax errors
sourceファイルのタイポかその辺のエラーじゃないか、ってことらしい。jekyllのsafeオプションを付けてビルドしてみる。以下のコマンドを実行

	jekyll build --safe

これも問題なし。


### Source setting
GitHubのビルドサーバーでは、sourceセッティングをオーバーライドするらしく、sourceの設定を弄っていると正しくビルドされないらしい。

sourceセッティングは変更していないので、これも原因ではなさそう。


### Submodules in Pages
リポジトリでサブモジュールを使っている場合、ビルド時に自動でそのサブモジュールをpullするらしい。
この時、サブモジュールのアクセス先がpublicではなくprivateだとアクセスできずに失敗するらしい。

サブモジュールは使っていないので、これも原因ではなさそう。


### Viewing build error messages
ビルドのエラーログを確認しろ、とのこと。

ローカル環境でビルドする以外の方法として、[Travis CI](http://travisci.org/)を使ってビルドエラーメッセージを確認することができるらしい。

手順は以下

* Gemfileを準備
リポジトリのルート直下に、ファイル名"Gemfile"で以下の内容を記述する。

{% highlight ruby %}
source 'https://rubygems.org'
gem 'github-pages'
{% endhighlight %}

* .travis.ymlの準備
テストサービスの設定に、ファイル名".travis.yml"で以下の内容を記述する。

{% highlight ruby %}
language: ruby
script: "bundle exec jekyll build"
{% endhighlight %}

この状態でGitHubへpushするとTravic CIでビルドジョブが実行される、らしい。


#### Travis CIのビルドメッセージを確認
Travis CIの画面から、以下のビルドメッセージを確認すると

{% highlight sh %}
$ bundle exec jekyll build
Configuration file: /home/travis/build/modalsoul/modalsoul.github.com/_config.yml
   Deprecation: Auto-regeneration can no longer be set from your configuration file(s). Use the --watch/-w command-line option instead.
    Source: /home/travis/build/modalsoul/modalsoul.github.com
    Destination: /home/travis/build/modalsoul/modalsoul.github.com/_site
    Generating...   Liquid Exception: invalid byte sequence in UTF-8 in post.html
error: invalid byte sequence in UTF-8. Use --trace to view backtrace
The command "bundle exec jekyll build" exited with 1.
Done. Your build exited with 1.
{% endhighlight %}

どうやらpost.htmlのパースでエラーになっているらしい。


## Liquid Exceptionの確認

### markdownパーサー

	Liquid Exception: invalid byte sequence in UTF-8

で、ググってみると

[Github Pages で page build failed エラー｜いがいが日記](http://igarashikuniaki.net/diary/20130519.html)
[Liquid Exception: Invalid Byte Sequence in UTF-8 in atom.xml｜T.I.D.](http://tokkonopapa.github.io/blog/2013/08/04/liquid-exception-error/)


markdownパーサーによってはビルドエラーになる場合があるらしい。
markdownパーサーを、"redcarpet"、"kramdown"、"rdiscount"と変えて試してみた結果、どのmarkdownパーサーでも同じくビルド失敗。

markdownパーサー依存の問題ではなさそう。


### 詳細なエラー発生箇所の確認
エラー発生箇所をもっと詳細に特定するために、post.htmlの中身を削ってビルド・エラーメッセージの確認を行う。
どうやら_includes/helpers/reference_thumbnail.htmlを使っている箇所が原因のよう。


## 解決
上記のファイルをエディタで開いてみると、文字コードがSHIFT_JISとなっていた。

これをUTF-8に変換してビルドしてみると、成功。

調べてみれば、なんてことない初歩的な問題でした。。












