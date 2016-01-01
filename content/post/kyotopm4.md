---
author: "kfly8"
date:   "2013-04-01 0:14:36+09:00"
linktitle: "#kyotopm にいってもくもくしてきた"
title: "#kyotopm にいってもくもくしてきた"
tags: ["perl"]
description: |
  そうだ、京都に行って来ましたー

  2013-03-30 @はてな
  http://www.zusaar.com/event/582004
---


そうだ、京都に行って来ましたー


2013-03-30 @はてな
http://www.zusaar.com/event/582004


<blockquote class="instagram-media" data-instgrm-captioned data-instgrm-version="6" style=" background:#FFF; border:0; border-radius:3px; box-shadow:0 0 1px 0 rgba(0,0,0,0.5),0 1px 10px 0 rgba(0,0,0,0.15); margin: 1px; max-width:658px; padding:0; width:99.375%; width:-webkit-calc(100% - 2px); width:calc(100% - 2px);"><div style="padding:8px;"> <div style=" background:#F8F8F8; line-height:0; margin-top:40px; padding:50% 0; text-align:center; width:100%;"> <div style=" background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACwAAAAsCAMAAAApWqozAAAAGFBMVEUiIiI9PT0eHh4gIB4hIBkcHBwcHBwcHBydr+JQAAAACHRSTlMABA4YHyQsM5jtaMwAAADfSURBVDjL7ZVBEgMhCAQBAf//42xcNbpAqakcM0ftUmFAAIBE81IqBJdS3lS6zs3bIpB9WED3YYXFPmHRfT8sgyrCP1x8uEUxLMzNWElFOYCV6mHWWwMzdPEKHlhLw7NWJqkHc4uIZphavDzA2JPzUDsBZziNae2S6owH8xPmX8G7zzgKEOPUoYHvGz1TBCxMkd3kwNVbU0gKHkx+iZILf77IofhrY1nYFnB/lQPb79drWOyJVa/DAvg9B/rLB4cC+Nqgdz/TvBbBnr6GBReqn/nRmDgaQEej7WhonozjF+Y2I/fZou/qAAAAAElFTkSuQmCC); display:block; height:44px; margin:0 auto -44px; position:relative; top:-22px; width:44px;"></div></div> <p style=" margin:8px 0 0 0; padding:0 4px;"> <a href="https://www.instagram.com/p/Xefv11oDDR/" style=" color:#000; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:normal; line-height:17px; text-decoration:none; word-wrap:break-word;" target="_blank">はてなのTシャツもらた！#kyotopm 発表done 懇親会へ</a></p> <p style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; line-height:17px; margin-bottom:0; margin-top:8px; overflow:hidden; padding:8px 0 7px; text-align:center; text-overflow:ellipsis; white-space:nowrap;">@kfly8が投稿した写真 - <time style=" font-family:Arial,sans-serif; font-size:14px; line-height:17px;" datetime="2013-03-30T09:23:43+00:00">2013  3月 30 2:23午前 PDT</time></p></div></blockquote>
<script async defer src="//platform.instagram.com/en_US/embeds.js"></script>
はてなのTシャツもらた。あざます！


今回のKyoto.pm は、
もくもくするハッカソンイベントでした。
自分は、数式のエディタ作ってみました。(ほとんどドキュメント読んでる時間でしたが...）

* http://mathml-kfly8.dotcloud.com/
* https://github.com/kfly8/Text2MathMLEditor


発表していた人の（わずかな）めも...
==================================================

Cinnamonの並列処理
--------
* id:shiba_yu36
* 並列処理のテストを書くのに、サーバ環境を２つ用意しないといけないことに気づいて、、
* vagrant


黒い画面でキャラを。
--------
* id:kiyotune
* https://github.com/kiyotune/my_logo/commits?author=kiyotune
* 動物の森関連の便利なのを子供に使って試してもらったお話

http://xpathfeed.com/
--------
* id:onishi
* https://github.com/onishi/xpathfeed

Net-Signalet
--------
* id:y_uuki
* https://github.com/y-uuki/Net-Signalet
* iperf
* opcontrol


以下、その日、発表で使った資料(にちょっと手を加えたもの)

-------------->8-----------------------------------------------------


自己紹介
=================================================

* kfly8
* 朝まで飲んで、品川発の新幹線に乗り込む。
* もしかして: ねむい
* 普段は、MFで、ソーシャルアプリ作ってます。
* よろしくお願いします。


つくったもの
=================================================

txtをMathMLに変換してくれるwebあぷぷ

* http://mathml-kfly8.dotcloud.com/
* https://github.com/kfly8/Text2MathMLEditor

仕組みの概要
-------------------------------------------------

* 変換部分は、 Text::ASCIIMathML にお任せ
* だいたいのLaTeXコマンドをサポートしている
* http://search.cpan.org/~nodine/Text-ASCIIMathML-0.81/lib/Text/ASCIIMathML.pm


Text::ASCIIMathML
-------------------------------------------------

    $parser     = new Text::ASCIIMathML();
    $mathMLTree = $parser->TextToMathMLTree('int_0^1 e^x dx');
    $mathMLTree->text();


動機を...
=================================================

weighted shuffle
-------------------------------------------------

    @weighed_list = [ { value => 'hoge', weight => 10 }, { value => 'fuga', weight => 100 } ];

    @weighted_shuffle_list = map { $_->{value} }
        sort { $b->{rand} <=> $a->{rand} }
        map {
            {
                rand  => rand($_->{weight}),
                value => $_->{value},
            }
        } @weighted_list;

ロジック
-------------------------------------------------

* weight が大きければ大きいほど、大きな乱数が得られる可能性が高いから、(一応)weightを考慮した配列にはなってる
* https://github.com/kfly8/p5-Data-WeightedRoundRobin/commit/fde15205df918e4130021c532b24418e1d81c8bb
* (カッとなって書いたので、D::WR に書くべきかどうかわからん)

weightの価値が知りたい
-------------------------------------------------

* この確率は、積分で表せるのだが...
* <span style="font-size: 150%"><b>*なんか数式で表したいなー*</b></span>
* これが、動機!!!!



MathMLって?
=================================================

* MathML は数式をマークアップするための XML 語彙
* 要は、xmlなので、DOM操作したり、検索しやすかったりする
* 非対応なブラウザがある

* ASCIIMathML
* http://www.ibm.com/developerworks/jp/xml/library/x-mathml3/
* http://en.wikipedia.org/wiki/ASCIIMathML

    ASCIIMathML is a client-side mathematical markup language for displaying mathematical expressions in web browsers.

* LaTeXMathML
* ASCIIMathMLの拡張のよう #TODO

数式を表す他の手段は？
=================================================

画像を使う
-------------------------------------------------

* Google Chart Tool
* <img src="http://chart.apis.google.com/chart?cht=tx&amp;chl=x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" alt="" />
* 画像で数式を表現

* CodeCogs
* 画像で数式を表現. 解像度を指定できる.

画像で数式を表した場合の問題点
-------------------------------------------------

* 機械に優しくない
* Liquidレイアウトしずらい


非対応ブラウザ対応
-------------------------------------------------

* MathJax ( http://www.mathjax.org/ )
* MathMLの非対応のブラウザでも、WebFont使ってみれるようにしてくれる君

* jsMath ( http://www.math.union.edu/~dpvc/jsMath/ )
* MathJax からフォークしたよう #TODO


今後どうしたいか
=================================================

* 変換精度を上げる
* エディタとしての使い勝手を上げる
* 例えば、オフラインで動かしたいので、JSで変換できるようにしたい
* 画像にしてダウンロード、シェアできるようにしたい。

* 数式をカジュアルに使いたい！


参考URL
=================================================

MathML
-------------------------------------------------

* LaTeXMathML
* http://math.etsu.edu/LaTeXMathML
* ASCIIMathML の拡張のよう #TODO

MathML ver2 @2003 -> ver3 @2010に移行しているので、どう違うのか調べる。 #TODO

変換ツール
-------------------------------------------------

* Pandoc
* Haskel でできた、いろんな形式をサポートしているのの。markdown -> tex とかできるよう。epub とかに変換できるよう。#TODO

* texのオンラインエディタ
* http://ja.numberempire.com/texequationeditor/equationeditor.php

* 手書きで数式を表せる
* http://detexify.kirelabs.org/classify.html


最後に
=================================================

もくもくコードが書けて楽しかったです。
畳でコード書けるっていいですね！
楽しかったです！

ありがとうございました!!


----------------8<---------------------------------


桜きれいだったー。

<blockquote class="instagram-media" data-instgrm-captioned data-instgrm-version="6" style=" background:#FFF; border:0; border-radius:3px; box-shadow:0 0 1px 0 rgba(0,0,0,0.5),0 1px 10px 0 rgba(0,0,0,0.15); margin: 1px; max-width:658px; padding:0; width:99.375%; width:-webkit-calc(100% - 2px); width:calc(100% - 2px);"><div style="padding:8px;"> <div style=" background:#F8F8F8; line-height:0; margin-top:40px; padding:50% 0; text-align:center; width:100%;"> <div style=" background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACwAAAAsCAMAAAApWqozAAAAGFBMVEUiIiI9PT0eHh4gIB4hIBkcHBwcHBwcHBydr+JQAAAACHRSTlMABA4YHyQsM5jtaMwAAADfSURBVDjL7ZVBEgMhCAQBAf//42xcNbpAqakcM0ftUmFAAIBE81IqBJdS3lS6zs3bIpB9WED3YYXFPmHRfT8sgyrCP1x8uEUxLMzNWElFOYCV6mHWWwMzdPEKHlhLw7NWJqkHc4uIZphavDzA2JPzUDsBZziNae2S6owH8xPmX8G7zzgKEOPUoYHvGz1TBCxMkd3kwNVbU0gKHkx+iZILf77IofhrY1nYFnB/lQPb79drWOyJVa/DAvg9B/rLB4cC+Nqgdz/TvBbBnr6GBReqn/nRmDgaQEej7WhonozjF+Y2I/fZou/qAAAAAElFTkSuQmCC); display:block; height:44px; margin:0 auto -44px; position:relative; top:-22px; width:44px;"></div></div> <p style=" margin:8px 0 0 0; padding:0 4px;"> <a href="https://www.instagram.com/p/XgnafPIDFU/" style=" color:#000; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:normal; line-height:17px; text-decoration:none; word-wrap:break-word;" target="_blank">手鞠鮨ー。</a></p> <p style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; line-height:17px; margin-bottom:0; margin-top:8px; overflow:hidden; padding:8px 0 7px; text-align:center; text-overflow:ellipsis; white-space:nowrap;">@kfly8が投稿した写真 - <time style=" font-family:Arial,sans-serif; font-size:14px; line-height:17px;" datetime="2013-03-31T05:09:11+00:00">2013  3月 30 10:09午後 PDT</time></p></div></blockquote>
<script async defer src="//platform.instagram.com/en_US/embeds.js"></script>

TODO: 教えてもらったラーメン屋に行く。
-----

* http://tabelog.com/kyoto/A2602/A260201/26006820/
* http://www.takakura-nijo.jp/



<b>*新しい年度が始まる！がんばろうー。*</b>



