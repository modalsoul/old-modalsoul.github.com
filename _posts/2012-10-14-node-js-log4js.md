---
layout: post
title: node.js+log4jsで、ログローテーションする
tags: node.js log4js
categories: report
comment: この記事は、node.jsとLoggingFrameworkのlog4jsを使ってのロギングとローテーションの方法についてのメモ書きです。
thumbnail: https://github.com/nomiddlename/log4js-node/blob/master/README.md
---

-----------------



## インストール

まずは、環境の準備です。

npmでlog4jsをインストールします。

{% highlight sh %}
$ npm install log4js
{% endhighlight %}

<hr />

## ログ出力のために必要な手順

*1. log4jsを読み込む

{% highlight sh %}
var log4js = require('log4js');
{% endhighlight %}

*2. loggerを取得する

{% highlight sh %}
var logger = log4js.getLogger();
{% endhighlight %}

*3. ログレベルを指定して、ログ出力する

{% highlight sh %}
logger.info('This is test.');
{% endhighlight %}

*4．実行結果

{% highlight sh %}
$ node logging-sample.js
[2012-10-14 14:04:49.197] [INFO] [default] - This is test.
{% endhighlight %}


出力形式を指定せずに実行すると、デフォルトで標準出力にログ出力されます。


<hr />

## ログをファイルに出力する手順

ログの出力先を標準出力からファイルに変更します。

*1. appenders.jsonを生成

{% highlight sh %}
appenders: [{
	"type": "file",
	"filename": "logging.log",
}]
{% endhighlight %}


*2. log4jsに設定を反映

{% highlight sh %}
log4js.configure({
	appenders: [{
	"type": "file",
	"filename": "logging.log",
	}]
});
{% endhighlight %}


*3. loggerを取得する

{% highlight sh %}
var logger = log4js.getLogger('file');
{% endhighlight %}

getLoggerの引数に、上記で設定したtypeを指定します。


*4．実行結果

実行すると、プロダクトと同じ階層に"logger.log"ファイルが生成され、以下のように出力されます。

{% highlight sh %}
[2012-10-14 18:50:33.876] [INFO] file - This is test.
{% endhighlight %}


### ログファイルの出力先を変更する
appendersのfilenameに、ファイルパスを含めて記載すると、出力先を任意に変更できます。

{% highlight sh %}
log4js.configure({
	appenders: [{
	"type": "file",
	"filename": "logs/logging.log",
	}]
});
{% endhighlight %}

上記の設定で実行すると、プロダクトと同階層にある"logs"ディレクトリ配下に、"logger.log"ファイルが生成され、ログが出力されます。

<i><b>ただし</b>、パスにしていしたディレクトリは自動で生成はされないので、あらかじめディレクトリを生成しておく必要があります。</i>


<hr />

## ログファイルのローテーションを設定する手順
<a href="https://github.com/nomiddlename/log4js-node/wiki/Date%20rolling%20file%20appender"><img title="Date rolling file appender ・ nomiddlename/log4js-node Wiki ・ GitHub" src="http://capture.heartrails.com/200x150/cool/1350212685200?https://github.com/nomiddlename/log4js-node/wiki/Date%20rolling%20file%20appender" alt="https://github.com/nomiddlename/log4js-node/wiki/Date%20rolling%20file%20appender" width="200" height="150" />
</a>
ログファイルを日次でローテーションするよう設定します。

*1. appenders.jsonを生成

{% highlight sh %}
appenders: [{
	"type": "dateFile",
	"filename": "logging.log",
	"pattern": "-yyyy-MM-dd"
}]
{% endhighlight %}


*2. log4jsに設定を反映する

log4js.configure({
	appenders: [{
	"type": "dateFile",
	"filename": "logging.log",
	"pattern": "-yyyy-MM-dd"
	}]
});

ここで設定している"pattern"は、ローテーションで生成されるログファイル名のpostfixです。


*3. loggerを取得する

{% highlight sh %}
var logger = log4js.getLogger('dateFile');
{% endhighlight %}

getLoggerの引数に、上記で設定したtypeを指定します。


*4. ローテーションの動作

上記設定の場合、初回起動時は"filename"の設定値の"logging.log"という名前のログファイルが生成されます。

そして午前0時に、現在の"logging.log"が、"logging.log<i>-yyyy-MM-dd</i>"にリネームされ新しい"logging.log"が生成されます。


<hr />

### patternの設定

patternの設定は、ローテーションされたファイルのpostfixになります。

patternの設定は、以下の書式が適用されます。

*・ yyyy : 西暦を4桁表示します。2桁表示するには"yy"を指定します。
*・ MM : 月
*・ dd : 日
*・ hh : 時間(24時間表示)
*・ mm : 分
*・ ss : 秒
*・ sss : ミリ秒
*・ O : タイムゾーン（大文字のOです）


これでnode.js+log4jsで、ログローテーションできるようになりました。

最後に、日次でローテーションするサンプル全コードを載せます。

<script src="https://gist.github.com/3888287.js"> 
</script>

この他にもlog4jsには色々な設定ができるようなので、機会を見て調べてみたいと思います。
