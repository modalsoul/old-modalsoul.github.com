---
layout: post
title: VMWarePlayerでUbuntu環境を立ててみた。
tags: Ubuntu, VMWare
categories: Environment
---
[VMWarePlayer](http://www.vmware.com/jp/products/desktop_virtualization/player/overview)で、[Ubuntu](http://www.ubuntulinux.jp/)環境を作ってみました。
-----------------

![VMWarePlayer](http://capture.heartrails.com/300x200/cool?http://www.vmware.com/jp/products/desktop_virtualization/player/overview)


##VMWarePlayerのインストール
###VMWarePlayerのダウンロード
何のことは無いです、↓からダウンロード
http://www.vmware.com/jp/products/desktop_virtualization/player/overview

今回ダウンロードしたファイルは、VMware-player-4.0.2-591240.exe

###VMWarePlayerのインストール
VMware-player-4.0.2-591240.exeを実行。

流れに沿って素直にインストール


##Ubuntuのインストール
###インストールメディアを挿入
細かいところは省きます。
手持ちのUbuntuのメディアは、Ubuntu11.04のCDがあったのでコレを使いました。
CDを挿入した状態で、VMWarePlayerを起動すると
CD上のUbuntuを選択できるので、インストール。

###Ubuntu11.10へのアップグレード
Updateじゃなくて、Upgradeなんすね。

恐ろしく時間掛かった。


##Gnome-panelのインストール
Ubuntu11.04ではGnome-classicを選択できたけど
Ubuntu11.10からは有無を言わさずUnityになるので、gnome-panelを使う。

{% highlight sh %}

$ sudo apt-get install gnome-panel

{% endhighlight %}


インストールが終わったら、一旦ログアウト。

ログイン画面の歯車をクリックして、Gnome-classicを選択する。

これでログインすると、Ubuntu11.10でもUbuntu11.04と同じデスクトップ環境になります。