---
layout: post
title: TouchUtilsを使ったAndroidテストでjava.lang.SecurityException: Permission Denialが起きたときの回避方法
tags: Android UI Test InstrumentationTestCase TouchUtils performClick
categories: Test Programing
comment: この記事は、以前書いた記事<a href="http://modalsoul.github.com/Test/Programing/2012/05/20/android-ui-test-InstrumentationTestCase/" >AndroidアプリのUIテスト-InstrumentationTestCase編-</a>の補足記事です。
thumbnail: http://modalsoul.github.com/Test/Programing/2012/05/20/android-ui-test-InstrumentationTestCase/
---
TouchUtilsを使ったAndroidテストでjava.lang.SecurityException: Permission Denialが起きたときの回避方法
-----------------

以前の記事では、InstrumantetionTestCaseとTouchUtilsを使ってのUIテストについて書きました。
最近、この方法でテストを実行してjava.lang.SecurityException: Permission Denialが発生して、テストに失敗する事象に悩まされたので、
回避方法を記録しておきます。

## なぜjava.lang.SecurityExceptionが発生するのか？

この問題が発生する正確な理由はわかっていません。

特定の端末(今回の場合、私が持っていたXperia arc android2.3.4)でのみ事象の発生が確認できました。

Xperia NX, MEDIASでは同じテストケースを実行してもこの現象は確認できませんでした。

これらの状況と、logcatで確認できたエラーログから察するに、
TouchUtilsは端末によっては利用できないよう制限されているようです。

そのため、テストケース中のボタンなどのViewのクリック動作がシミュレートできずにテストが失敗したようです。


## TouchUtilsを使わずにテストする方法

以前紹介した方法では、ユーザのViewのクリック動作を以下のような方法でシミュレートしました。

### Activityの取得
<script src="https://gist.github.com/2758303.js?file=getCurrentActivity.java">
</script>

### Viewを取得して、初期状態を確認。
<script src="https://gist.github.com/2758332.js?file=getAndCheckView.java">
</script>

### ボタンのクリックをシミュレート。
<script src="https://gist.github.com/2758332.js?file=getAndCheckView.java">
</script>


上記のクリックのシミュレートに使用したTouchUtilsの代わりに、[performClick()](http://developer.android.com/reference/android/view/View.html#performClick())を使います。

※この場合、clickPerformは明示的にUIスレッドで実行する必要があります。

<script src="https://gist.github.com/3531257.js?file=performClick-sample.java">
</script>


