---
author: "kfly8"
date:   "2014-10-07 16:28:16+09:00"
linktitle: "isuconの予選で失格してきた"
title: "isuconの予選で失格してきた"
tags: ["isucon"]
description: |
  運営の方々、楽しいイベントをありがとうございました！！
  kuraチームのid:karupanerura, id:masasuz ありがとうございました！
---

牡蠣途中



運営の方々、楽しいイベントをありがとうございました！！
kuraチームのid:karupanerura, id:masasuz ありがとうございました！

isucon中にやったこと

## サマリーテーブルを使って、ログインの成否を判定するようにした

https://github.com/karupanerura/isucon2014-elimination/commit/b81dcd38b7201c838ab49501a0a91984b13361b4
https://github.com/karupanerura/isucon2014-elimination/commit/2daf40a936fc541841cc50fcb2c408fee7530444


## サマリーテーブルを使って、集計を出すようにした

https://github.com/karupanerura/isucon2014-elimination/commit/f4bd9e12c1d17ef1dab601b745c82daed134e504

* 紆余曲折があり、本戦中にちゃんとやり切れなかった。

* 競技終了直前に気づいた不具合

https://github.com/karupanerura/isucon2014-elimination/commit/f4bd9e12c1d17ef1dab601b745c82daed134e504#diff-4f430011a6a953bf6b408bc9906c5478L171

```perl
push @user_ids => $row->{login};


$row->{login}...


＿人人人人人人人人人＿
＞　idじゃない！！　＜
￣Y^Y^Y^Y^Y^Y^Y^Y￣
```

TODO

## userデータをオンメモリーにした

https://github.com/karupanerura/isucon2014-elimination/commit/2ee1d266ea6f076556e578f17ed6bf4c6b8c60b1

* evalとか男前な実装になってすごいテンパっているなってなった
* せめてこう一発で書きたかった。。
  * https://github.com/karupanerura/isucon2014-elimination/commit/7c6e6d2a1df780f6cfaa2d0c66f8797a85cb9ad2

TODO

## slack

https://github.com/karupanerura/isucon2014-elimination/issues/5

捗った

TODO

