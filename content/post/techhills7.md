---
author: "kfly8"
date:   "2013-11-13 01:25:14+09:00"
linktitle: "#techhills #7にいってきたー"
title: "#techhills #7にいってきたー"
tags: ["game"]
description: |
  CROOZさん主催のtechhillsに初めて参加させてもらった。
  いろんなゲームエンジンの話で興味深く聞かせてもらいました！
  ありがとうございました！
---

CROOZさん主催のtechhillsに初めて参加させてもらった。
いろんなゲームエンジンの話で興味深く聞かせてもらいました！
ありがとうございました！

* http://www.techhills.net/
* http://atnd.org/events/44622
* https://twitter.com/search?q=%23techhills&src=hash

かんたんなメモ!!資料はあげてもらえたら随時更新で。

# Unity2D/New Unity GUI(uGUI)

* 物理エンジンにBox2Dを使っている
* Spriteが主にゲーム用に用意されている

https://twitter.com/kyusyukeigo/status/400264293565755392

# Aiming/Unity

http://www.slideshare.net/KatsutoshiMakino/aiming

* MMORPGげんとうせんきグリフォンを作ったときの話
  * http://griffon.sega-aiming.com/
* メモリ使用量
  * gcでうわーってなるのは、リソースを大量に切り替えた時やアンロードしたとき
  * addClipしたのを使い回しして、節約
* UI
  * NGUIを使用
  * 日本語の為に、ビットマップフォントを使用
  * ダイナミックフォントにしたいそう
* リソースのダウンロード
  * asset bundle
  * LoadFromCacheOrDownload

# cocos2d-x

* C++,Lua,jsなどをサポートするOSSゲームフレームワーク
* CocosBuilder/SpriteBuilderなどのUIエディタを使うといいんじゃないか

# enchant.js

* コードたのしいよ

# Playground KLab

* https://github.com/KLab/PlaygroundOSS

## Playground : 描画周りの内部の仕組み

http://www.slideshare.net/RomainPiquois/playground-28101668

* いろいろ難しいところをいい感じにやってくれている？そう。
* カメラに映ってないものは、描画しない（カリング）
* ピクセルの書き換えを減らす
  * 透明でない要素の並びを、近くから遠くに並び替える(重なっている部分の書き換えは最前面だけということだろう）
  * 透明な要素の並びを、遠くから近くに並び替える
  * シェダー？でソート
  * モバイル2D環境だと、シェダーは変わらないゲームが多く、透明でない要素は少ないのが特徴
* 描画コールを減らす
  * 処理をまとめる/状態が同じものでバッチ処理
  * 状態に関するツリー構造のようなのを管理し？、変わった所だけ変更を行うようにする

## Playground : 音ゲーのAndroid対応の話

* http://www.slideshare.net/KeiNakazawa/playgroundandroid-pfandroid


# ゲームエンジンの比較の話

XXX ベンチ公開されるといいなー


---------

[![](http://instagram.com/p/gnj367IDL0/media/?size=l)](http://instagram.com/p/gnj367IDL0/)

[わーきれいだなー(棒
ゲームエンジン勉強会たのしかった！](http://instagram.com/p/gnj367IDL0/)


