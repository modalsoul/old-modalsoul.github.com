---
layout: post
title: Android端末をUSBデバッグ接続してもMacのDDMSで認識されない問題でハマった件について
tags: Android DDMS Mac
categories: report
comment: この記事は、MacでAndroidアプリ開発するべく開発環境を構築したときのメモです。特にハマった現象について、原因と対処法を記録しておきます。
thumbnail: http://developer.android.com/index.html
---


## 現象について
Android端末をUSBケーブルでつないでデバッグ接続しても、EclipseのDDMSで認識されませんでした。

PCは、Core2世代のMac book air

Android端末は、docomo版Sony EricssonのXperia arc

<hr />
## 原因
端的に言うと、MBAにインストールされていたEasyTetherのドライバと競合していた為でした。

<a href="https://play.google.com/store/apps/details?id=com.mstream.easytether_polyclef&hl=ja">
<img title="Now Capturing..." src="http://capture.heartrails.com/300x200/cool?https://play.google.com/store/apps/details?id=com.mstream.easytether_polyclef&hl=ja" alt="https://play.google.com/store/apps/details?id=com.mstream.easytether_polyclef&hl=ja" width="300" height="200" />
</a>

<hr />
## 対処法
EasyTetherのドライバを削除することで、DDMSでAndroid端末が認識されない問題は解消できます。


>「/System/Library/Extentions」をFinderで開き、「EasyTetherUSBEthernet.kext」を削除する


競合そのものを解消する方法ではないので、EasyTetherが使えなくなりますが、Android2.3以降？の端末では、OSの機能としてテザリングがサポートされているようなので影響は少ないかな、と。

[こちら](http://gadget-shot.com/news/5757)の記事を参考しました。

