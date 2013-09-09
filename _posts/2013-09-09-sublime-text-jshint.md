---
layout: post
title: Sublime text2でjshintプラグインを使う
tags: SublimeText jshint JavaScript
categories: environment
comment: この記事は、Sublime Text2にJSHintプラグインをインストールしてハマった時のメモ書きです。
thumbnail: https://dl.dropboxusercontent.com/u/2388316/screenshots/sublime-jshint.png
---

## インストール手順
基本的にSublime-JSHintのGithubのREADMEの通りです。
以下の手順はMacの場合のものです。

### node.jsのインストール
JSHintを実行する為に、Sublime Textとは別に[node.js](http://nodejs.org/#download)がインストールされている必要があるようです。

### Sublime-JSHintのインストール
[Sublime Text package manager](https://sublime.wbond.net/installation)を使った方法と、マニュアルでのインストール方法がありますが、今回はpackage managerを使ってインストールしました。

* Cmd+Shift+P
* install と入力し、 Package Control: Install Package を選択
* js gutter と入力し、 JSHint Gutter を選択

### 動作確認
適当なJavaScriptファイルをSublime Textで開き、

* Cmd+Shift+P
* JSHintを選択

これでラインナンバーの左側にJSHintの警告マークがでればOKですが、手元の環境では以下のようなエラーが出てしまいました。

![Sublime-JSHint error popup](/img/20130909/sublime-error-popup.png)

### node_pathの設定
上記のエラーはnode.jsのパス設定がうまくいっていないためのエラーのようです。
GithubのREADMEでもこういった現象が起きることが書かれていました。[Oh noez, command not found!](https://github.com/victorporof/Sublime-JSHint#oh-noez-command-not-found)

READMEの記述にしたがいnode_pathを設定します

* Cmd+Shift+P
* jshint と入力し、Set node Path を選択

JSHint.sublime-settingsが開き、デフォルトの設定は以下のようになっています。

	{
	  // Simply using `node` without specifying a path sometimes doesn't work :(
	  // https://github.com/victorporof/Sublime-JSHint#oh-noez-command-not-found
	  // http://nodejs.org/#download
	  "node_path": "/usr/local/bin/node",

	  // Automatically lint on edit (Sublime Text 3 only).
	  "lint_on_edit": false,

	  // Automatically lint when a file is saved.
	  "lint_on_save": false
	}

上の"node_path"がnode.jsのパス、真ん中の"lint_on_edit"は編集時の自動lintの設定、下が保存時の自動lintの設定。

ちなみに、私の環境ではnode.jsのインストールパスは上記のデフォルトパスと同じになっていました。
設定からすると動くはずの用に思えます。

### リンク元のパスを設定
"sublime node.js not found"でググってみたところ、Sublime-HTMLPrettifyに参考になりそうなissueがありました。

[Node.js was not found when it is there](https://github.com/victorporof/Sublime-HTMLPrettify/issues/53) 

このissueのコメントを漁っていくと、[それっぽいコメント](https://github.com/victorporof/Sublime-HTMLPrettify/issues/53#issuecomment-24030863)がありました

このコメントによると、
/usr/local/bin/node のリンク元　/usr/local/Cellar/node/0.10.15/bin/node と設定したらうまくいった、らしい


これを参考に、ls -l /usr/local/bin/node の結果を、JSHint.sublime-settingsのnode_pathに設定し、再起動したらJSHintが動きました。

とりあえずめでたしめでたし


ただ、このissueのコメントだと同じくMacでも/usr/local/bin/nodeで動いたケースもあるようなので、詳細は不明ですね。。









