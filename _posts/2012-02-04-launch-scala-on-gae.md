---
layout: post
title: Gitlabを立ててみた。
tags: Git Gitlab Ubuntu
categories: record
---
[GitLab](http://gitlabhq.com/)を立ててみた。
-----------------

![]()

[こちら]()と[こちら]()の記事を参考にしました。


##開発環境の準備
###Google Plugin for Eclipseのインストール
EclipseにGoogle Pluginをインストール
Help->Install New Softwareから↓をインストール

http://dl.google.com/eclipse/plugin/3.6

そしたら、
{% highlight sh %}
・・・
 requires 'bundle com.android.ide.eclipse.adt 12.0.0' but it could not be found
・・・
{% endhighlight %}
とのこと

eclipse.adt 12.0.0をインストールします。
[こちら](http://developer.android.com/sdk/eclipse-adt.html#installing)を参考に。

https://dl-ssl.google.com/android/eclipse/


ADTのインストール後、再度Google Pluginをインストール

##ScalaのController/Serviceを自動生成する
###antのインストール
http://www.javadrive.jp/ant/install/index1.html


ant のbuile.propertie のパスは相対パス

Unable to locate tools.jar.
http://takuan93.blog62.fc2.com/blog-entry-13.html



http://stackoverflow.com/questions/3243984/scala-ide-error-projectname-is-not-a-scala-project

