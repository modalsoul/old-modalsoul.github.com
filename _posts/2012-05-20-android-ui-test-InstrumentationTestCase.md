---
layout: post
title: AndroidアプリのUIテスト-InstrumentationTestCase編-
tags: Android UI Test InstrumentationTestCase
categories: Test Programing
---
AndroidアプリのUIテスト-InstrumentationTestCase編-
-----------------
複数のActivityに跨る画面遷移のテストを調べて、サンプルを作ってみました。

サンプルのAndroidプロジェクトとテストプロジェクトは以下に置きました。

サンプルAndroidアプリ
[uiTestSample](https://github.com/modalsoul/uiTestSample)

サンプルテスト
[uiTestSample-test](https://github.com/modalsoul/uiTestSample-test)


参考にしたのは以下のページ
[How do you test an Android application across multiple Activities?](http://stackoverflow.com/questions/1759626/how-do-you-test-an-android-application-across-multiple-activities)

<I>※：参考ページでは、JUnit4を使っていますが、ここではJUnit3を使っているため一部違っています。</I>

<hr />

## サンプルアプリの動作について
このサンプルでは、アプリの起動後の画面にボタンが表示され、そのボタンをクリックすると別画面に遷移します。

<h3>起動後の画面</h3>

<a href="http://www.flickr.com/photos/modalsoul/7233426834/" title="uiTestSample-main by modal_soul, on Flickr">
<img src="http://farm6.staticflickr.com/5343/7233426834_d72dda0713.jpg" width="500" height="492" alt="uiTestSample-main">
</a>

<h3>ボタンクリックで遷移後の画面</h3>

<a href="http://www.flickr.com/photos/modalsoul/7233469296/" title="uiTestSample-transited by modal_soul, on Flickr">
<img src="http://farm6.staticflickr.com/5196/7233469296_d04794833e.jpg" width="500" height="492" alt="uiTestSample-transited">
</a>


<hr />

## テストの概要
このサンプルでは、アプリ起動後の画面のTextViewの内容を確認。ボタン1のクリックをエミュレートし、画面遷移後の画面のTextViewの内容を確認しています。

### InstrumentationTestCase
InstrumentationTestCaseを継承したクラスを作成します。
getInstrumentation()を使うことで、現在有効になってるアクティビティを捕捉し、TouchUtilsでユーザ操作をエミュレートします。


## テストの流れ

### 捕捉するインテント情報を設定

<script src="https://gist.github.com/2758292.js?file=getInstrumentation.java">
</script>

### 存在するInstrumentation.ActivityMonitorがヒットするまで待ちます。タイムアウト時間を設定。

<script src="https://gist.github.com/2758303.js?file=getCurrentActivity.java">
</script>

### Viewを取得して、初期状態を確認。

<script src="https://gist.github.com/2758332.js?file=getAndCheckView.java">
</script>

### ボタンのクリックをエミュレート。
<script src="https://gist.github.com/2758337.js?file=emulateButtonClick.java">
</script>

### 再度Activityを取得。
<script src="https://gist.github.com/2758348.js?file=getCurrentActivity2.java">
</script>

### 画面遷移後の表示を確認。
<script src="https://gist.github.com/2758351.js?file=getAndCheckView2.java">
</script>

## まとめ
TouchUtilsを使えば、このほかにもドラッグ、スクロール、長押し等色々エミュレートでき、UIスレッドを意識しないで書けるので、アプリのハッピーパスのテストくらいであれば簡単に書けそうです。

