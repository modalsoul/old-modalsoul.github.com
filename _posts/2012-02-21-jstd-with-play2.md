---
layout: post
title: 【意訳】Play Framework 2.0とJsTestDriverによるJavaScriptテスト。
tags: Play2 JsTestDriver JavaScript Test
categories: Test
---
【意訳】Testing javascript with Play Framework 2.0 and JsTestDriver
-----------------

![Testing javascript with Play Framework 2.0 and JsTestDriver](http://capture.heartrails.com/300x200/cool?http://www.eishay.com/2012/02/testing-javascript-with-play-framework.html)


[Testing javascript with Play Framework 2.0 and JsTestDriver](http://www.eishay.com/2012/02/testing-javascript-with-play-framework.html)を適当翻訳でご紹介します。

元記事は[こちら](http://www.eishay.com/2012/02/testing-javascript-with-play-framework.html)

<hr />

qwallet.comでは、バックエンドにScala on Play2.0を使った広大なjavascriptアプリケーションを構築しています。

Scalaの開発にIntelliJ IDEAを使うようになってからは、JavaScriptでも同じように使っています。

これにより、IntelliJとJsTestDriverの統合による恩恵を受けています。

JavaScriptアプリでは、常に3つのブラウザを対象にテストをしています。
<img src="http://3.bp.blogspot.com/-IYdsLWKTgmA/Tzy0yd1OVUI/AAAAAAAABK4/2u5mI8xOwu4/s1600/server.jpg" width="80%" height="80%">

テストコンソールにはテスト結果と、ブラウザからフェッチされたコンソール出力を確認できます。
<img src="http://4.bp.blogspot.com/-CDyQpVkCRhU/Tzy1FNhDTiI/AAAAAAAABLA/ZYSJTpS4iXw/s1600/test_rez.jpg" width="80%" height="80%">

上の試験例では、FireFox, Chromeのテストは通っていますが、Safariでは失敗しています。
サンプルでは３つのテストケースがあり、拡張した1つは2つのテストメソッドを持っています。
もしエラーが発生した場合、スタックトレースがクリッカブルになり、IntelliJはエラーの行右側に飛びます。

テストはとても速いですが、この小さなサブセットのためにChromeでは6msかかっていますが、まだ改善の余地があります。


Play2.0の親切なところは、JavaScriptファイルの自動コンパイルです。
JsTestDriverのdevモードでは、ライブラリファイルとテストケースをブラウザに送り、アプリケーションファイルはPlay2.0アプリケーションによって送られます。
Playは、ファイルの変更を検知して自動でリコンパイルします。
テストを書くときに見逃しているかもしれないものをコンパイラがキャッチしてくれ、結果としてカバレッジが向上します。
<script src="https://gist.github.com/1843026.js?file=gistfile1.txt">
</script>


CIモードでは、JavaScriptファイルは1度コンパイルされておりPlayを経由して送られる必要がない。
この方法はとても早く、Playがバックグラウンドで実行されている必要性を低減してくれます。

qwallet.comの継続的デプロイの運用では、アプリケーションの全ての部分をテストします。
JsTestDriverはクロージャーコンパイラーとバンドルされ、fast devモードユニットテストとテスティングストラテジーのコンポーネントとして提供されています。



ところで、もし興味があったら、jobs@qwalletにメールを送ってくれ。
