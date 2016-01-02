---
author: "kfly8"
date:   "2013-11-30 20:03:31+09:00"
linktitle: "isucon に負けて悔しいので、問題を速く解決するためのパターンを考えてみた"
title: "isucon に負けて悔しいので、問題を速く解決するためのパターンを考えてみた"
tags: ["isucon"]
description: |
  isucon決勝からもうだいぶ日が経ってしまいました、、その間、負けて、id:karupanerura先生に、げきおこされる夢も見ました。
---

isucon決勝からもうだいぶ日が経ってしまいました、、その間、負けて、id:karupanerura先生に、げきおこされる夢も見ました。


振り返ります。

[isucon](http://isucon.net/)でスコアを残しているチームは技術力もさることながら、
問題解決能力が高いと思うのです。もの凄く。

近づくにはどうすればいいんだろうと、頭の整理のために、
自分が普段感じている問題解決のパターンを書いてみました。
特にisuconの反省なので、与えられた問題を良い感じに速くするための解決方法寄りだと思います。


# パターン

---

* 処理((仕事に置き換えても面白いかも？))しない
* 処理したらメモしておく
* 処理の下準備をする
* 処理をまとめる
* 処理を少なくする

---

* 処理を外にお願いする
* 必要な分だけ、処理する
* 処理を得意な人にお願いする
* 処理関係者を減らす

---

* 処理するヒトを増やす
* 処理できるヒトを良くする

---

* 仕様を仕様を削る

---



「isucon知識足りなくて、負けたわー、じゃー、ほげほげ勉強しよう！」っていうんじゃ、
思考停止だと思っています。

また知らない技術領域が出て来ても、
どうにかこうにか対応できるようになりたいなーと思った感じです
（知識不足は、ちゃんと補完しなきゃですが、、）


箇条書きで終わりもアレなので、
良い例かは分からないですが、パッと思いつく例を書いてみます。


### 処理しない

一番、大事なことだと思います。処理しないで、済むなら、処理しない。
やらないですむなら、やらない。

```
1 + 1
```
たたきこみ

### 処理したらメモしておく

今回のisucon決勝なら、画像投稿時、ないし、初回呼び出し時などで、キャッシュをするなど。

どこでキャッシュするか、いつキャッシュするか、メモしたものをどう更新するかは色々ですが、やったことは、またやりたくないというのは（領域が許せば）良い。

### 処理の下準備をする

indexや、辞書作りなどの学習

### 処理をまとめる

都度の処理呼び出しコストを減らす。
描画処理を減らすために、DOMに書き出さないなど。

### 処理を少なくする

処理を無くせはしないけど、少なくすることはできるっていうのはある。
処理内容が減れば、大小あれど、一応嬉しい。

```
# あるある
$dbh->query() for 1..100
```

---

ここから、ちょっと、トレードオフが大きくなりそうな、パターン。


### 処理を外にお願いする

処理が完了するまで、次の処理が出来ないのはもったいない
なので、他の人にお願いしたくなったりすることもある

### 必要な分だけ処理する

重い処理の割には、意外と使われなくて、事前準備してなくて構わないというのはあるなと。
そしたら、必要になったときに、処理すればよい。

### 処理を得意な人にお願いする

言語レベルか、機械レベルか、ソフトウェアか、、など。
これで良さそうな判断ができるかどうかは知識量に凄い左右されると思っている。

それでも得意な人にお願いしたいというのは変わらないはず。
使いこなせる武器が多いのは、視点が高くなる。

### 処理関係者を減らす

全てを一番ベストな人にお願いしても、１０００箇所で処理が行われるとしたら、
その処理の受け渡しのコストが目につくかもしれない。


### 処理するヒトを増やす

* 1人でやるより、2人で処理出来る方が捗ることもある。
* スケールアウト

### 処理できるヒトを良くする

* スケールアップ

### 仕様を削る

* その仕様、、、、その仕様、、い、、るかも、、いや、いら、らないかも、、　っていうのは、バッサバッサ切れればいいね。
* 難しい処理の方がお金になったりすることもあるから、難しいね。でも無駄なのもあるよね。
* エンジニアから、一歩踏み出すのも、問題解決する為の一つの手段。

# 最後に

id:karupanerura先生、id:masasuz先生、牡蠣鍋打ち上げしましょう> <

KAYACさん、LINEさん、isuconありがとうございました！！！！！


[![](http://instagram.com/p/gvFRWwIDIR/media/?size=l)](http://instagram.com/p/gvFRWwIDIR/)







