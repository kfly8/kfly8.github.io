---
author: "kfly8"
date:   "2013-11-16 0:53:40+09:00"
linktitle: "大規模なjsってどうして書きにくい？"
title: "大規模なjsってどうして書きにくい？"
tags: ["javascript"]
description: |
  大規模なjsってどうして書きにくい？

  そんな話を、id:karupanerura と[#isucon反省会](http://www.zusaar.com/event/1737003)帰り、東海道線で話した
---

フロントサイドでの話です。

そんな話を、id:karupanerura と[#isucon反省会](http://www.zusaar.com/event/1737003)帰り、東海道線で話した

２人で話した答え:

* 「パッケージの責任範囲を小さくするベストプラクティスが分からない」
* つまりは「コードを読む範囲を小さくしたい」

所感:

* "よしなに"やってくれるって、"なんかこわい"
  * jsのフロントサイドのライブラリって何となくそういうの多い気がする
  * 作るのも大変
* CPANのように客観的によくテストする仕組みがあれば、使う人も作る人も幸せになれる？
* 一つのことを上手くやる小さなモジュールを組み合わせたい

# どうやって解決するか？

* モジュールを決められた仕様で書いて、requireして使うcommonjsライクがいいんでは？と話した
  * モジュール毎にHTTP Requestするのはいやなので、concatとかもしやすくしたい
  * 【追記】せっかく非同期に読み込むこともできるのだから、依存関係解決しながらいい感じに読み込むめばいいのか、、と思うと、require.jsでいいのか 、、このサポートのことは一旦忘れよう、

例えば、以下のようにfoo,bar.jsというモジュールを作り、、
```sh
# lib/foo.js
exports.foo = function() {...};
# lib/bar.js
exports.bar = function() {...};
```

package.jsonやcpanfileのようなpackagefileを書くと、、
```sh
require lib/foo.js;
require lib/bar.js;
```

gruntのようなビルドツールでビルドして、、
```sh
# generate file
SHELL% BUILD aaa # generate aaa.js
```

requireして、各モジュールが使える
```html
# some.html
<script src="aaa.js"></script>
<script>
(function() {
    var foo = aaa.require('foo'); # source.mapも利用しやすいはず
    var bar = aaa.require('bar');
})();
</script>
```
テストは、npm test とか

# 最後

上のようなことなら、個人でやる分には素直に書けるけど、、最初の命題に対するアプローチどうなんだろうか、、？jsの何が一番いけてない？

すばらしいライブラリやaltjsがあるというのに、jsを書くというアプローチ、、
悪い言い方ですが、臭いモノの蓋を開けても、そんなに臭わないようにしたいなーというそんな願望の現れです。かっこつければ、js書くのが楽しくなれば、それはそれで意義深いかも？！という感じでございます。

以上！


