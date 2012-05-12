---
layout: post
title: スクラム道 Full Boost 「青物横丁制圧作戦」に参加しました。
tags: Agile
categories: report
---
[スクラム道 Full Boost 「青物横丁制圧作戦」](http://www.taoofscrum.org/contents/post/240)に参加しました。
-----------------

<a href="http://www.taoofscrum.org/contents/post/240">
<img title="スクラム道 Full Boost 「青物横丁制圧作戦」 | スクラム道" src="http://capture.heartrails.com/400x300/cool/shorten?http://www.taoofscrum.org/contents/post/240" alt="http://www.taoofscrum.org/contents/post/240" width="400" />
</a>
<hr />
この記事では、選手として参加させていただいて感じたことと、自分が壇上で行った質問や意見の補足をメインに書いています。
イベント自体については、他方々の記事にとてもわかりやすくまとめられていますので、そちらを参考にして頂ければと思います。

** [スクラム道フルブーストに参加してきた #scrumdo #nawoto_girls - Shinya’s Daily Report]()
** [スクラム道フルブーストに参加しました。 #scrumdo - k_0kamotoの日記]


## スクラム道とは？
{% highlight sh %}
スクラム道は、スクラムを中心にアジャイルを探究するための場です。
アジャイルは本を読んだだけでは大事な部分は伝わらず、そのため、せっかく現場でスクラムやアジャイルのプラクティスを導入しても上手く成果が出ないという悩みを良く聞きます。
スクラム道では、毎回、参加者と共に一つのテーマを深く掘り下げて探究し、現場の悩みを少しでも解消する事を目的にしています。
{% endhighlight %}
<I>スクラム道HPより引用</I>
<br />

<p>スクラム道へは、2011/12/26に読み手@ryuzeeさんで行われたスクラム道場.08[「Doneの定義　虎の巻」](http://www.taoofscrum.org/contents/post/214)、2012/03/28に読み手@nawotoさんで行われた[「良く書けたインセプションデッキとは？」](http://www.taoofscrum.org/contents/post/220)の2度参加し、
今回のスクラム道 FullBoostが3度目の参加となりました。</p>

<hr />

## 今回のテーマ
今回はスクラム道10回記念ということで、過去に行われた5テーマから参加者の投票で再演を行う形式で、@imagireさんによる「プロダクトバックログ」の再演となりました。

<div style="width:425px" id="__ss_9027962"> 
<strong style="display:block;margin:12px 0 4px">
<a href="http://www.slideshare.net/imagire/07-9027962" title="スクラム道場.07" target="_blank">スクラム道場.07</a>
</strong> 
<iframe src="http://www.slideshare.net/slideshow/embed_code/9027962" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no">
</iframe> 
<div style="padding:5px 0 12px"> View more 
<a href="http://www.slideshare.net/" target="_blank">presentations</a>
 from 
<a href="http://www.slideshare.net/imagire" target="_blank">Takashi Imagire</a> 
</div> 
</div>

読み手の@imagireさんによる発表の後、「議論に参加したい」「プロダクトバックログについて悩んでいる」人が選手として登壇。

選手として登壇した人には、マイクロソフトさんからプランニングポーカーがもらえる！とのことだったので、物欲に駆られて登壇してきました。

### ステマ
<blockquote class="twitter-tweet" lang="ja">
<p>おぉ！マイクロソフトさんからプランニングポーカーの差し入れだ!! 
<a href="https://twitter.com/search/%2523scrumdo">#scrumdo</a>
</p>&mdash; imae masatoshiさん (@modal_soul) 
<a href="https://twitter.com/modal_soul/status/200892170767577088" data-datetime="2012-05-11T10:16:40+00:00">5月 11, 2012</a>
</blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8">
</script>

<hr />

## ディスカッションRound1

プロダクトバックログに関して「ストーリーポイントを見積もる際の基準ストーリーが、後々基準として不適格だった場合どうするか？」と質問させていただきました。

あの場の発言だけでは説明しきれていいなかった部分も多々あるので、ここではこの質問の件を補足したいと思います。

### 前提として
1つのプロジェクトが開始されるにあたり、プロダクトバックログ(以下、PBL)が作成されます。
このPBLの各項目にストーリーポイント(以下、SP)を見積もる際、基準となる項目(以下、基準ストーリー)を選び、その他のPBLの各項目について見積もると思います。
<I>私のチームの場合、ストーリーポイントが２と５に相当しそうな基準ストーリーを選び、見積もりを行っています</I>

これ以降、新たにPBLに追加された項目についても同じ基準ストーリーを用いて見積もりを行えば、以前見積もった項目と大きさの比較が可能であると言えます。
<I>例）10kgは1kgの10倍の重さです。なぜなら同じ基準（＝単位、ここでいうkg）を用いているから</I>


### もしも基準が変わると？
基準ストーリーを実際に消化したところ、当初見積もっていたポイントよりもとても大きかったorとても小さかった、とします。

そうすると、その基準を用いた見積もりの結果の信頼性に疑問が出てきます。

<I>例えば、SPが２と見積もっていた基準ストーリー「A」が実はSPが10に相当するボリュームで、SPが5と見積もっていた基準ストーリー「B」がSP2のボリュームだった場合</I>

<p>ある項目「X」の見積もりを、SP2の基準ストーリー「A」以上SP5の基準ストーリー「B」未満だからSP3と見積もった</p>
<p>基準ストーリーのSPの変化を適用すると、ある項目Xの見積もりは、SP10以上SP2未満となってしまい、この項目Xの見積もりが破綻してしまいます。</p>


このような場合、新たに基準を設けて再見積もりを行えば問題は解消されると思います。

ただそうすると、各スプリントで計測したベロシティは、基準が異なるので単純な比較が行えなくなります。

### ベロシティって重要ですか？
ベロシティを使う目的は様々あると思いますが、ざっと挙げると以下のような感じでしょうか。

** スプリントで消化するSPの見積もり
** チームの成長度合い


ディスカッションでの質問やツイートの反応にもありましたが、決して<strong>ベロシティを人事評価に使いたい訳ではありません。</strong>この所は、特に強調しておきたいと思います。


ただ、現実問題としてアジャイルなプロセスを取り入れたことによる効果を、アジャイルに対する本質的な理解を持たない人に説明することを考えたときどうでしょう？
「価値判断で計測すればいい」という意見は正しいと思いますが、スプリント毎のROIを正しく計測するのは難易度が高いと思います。




## ディスカッションRound2

## 最後に
今回この様な有意義なイベントに参加させていただき、スクラム道スタッフの皆様、選手・参加者の皆様、バンダイナムコ様、マイクロソフト様、ありがとうございました。