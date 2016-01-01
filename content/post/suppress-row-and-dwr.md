---
author: "kfly8"
date:   "2013-01-23 00:51:35+09:00"
linktitle: "suppressしたrow objの存在しないkeyにアクセスして、Data::WeightedRoundRobin の初期値が使われて...orz  "
title: "suppressしたrow objの存在しないkeyにアクセスして、Data::WeightedRoundRobin の初期値が使われて...orz  "
tags: ["perl"]
description: |
  たった今、かなしい！となったので勢いに任せて書きます.

  普段、Data::WeightedRoundRobin のSugarを次のような感じで
  exportして使っています.
---


たった今、かなしい！となったので勢いに任せて書きます.


普段、Data::WeightedRoundRobin のSugarを次のような感じで
exportして使っています.

```perl
use Data::WeightedRoundRobin;

sub list2dwr (&@) {## no critic
    my $code = shift;
    return Data::WeightedRoundRobin->new([ map { $code->($_) } @_ ])
}

my $dwr = list2dwr {
    +{
       key    => $_,
       value  => $_ * $_,
       weight => $_,
    }
} qw/1 2 3 4/;
```

普段、Teng のsuppress_object_creation も、データが多くなるってとき、
使っています.

```perl
my @rows = do {
    my $iter = model('DB::Hoge')->search(+{ user_id => $args->{user_id} });
    $iter->suppress_object_creation(1);
    $iter->all;
};
```


で、はまったコードはこれ.


```perl
my @rows = do {
    my $iter = model('DB::Hoge')->search(+{ user_id => $args->{user_id} });
    $iter->suppress_object_creation(1);
    $iter->all;
};

my $dwr = list2dwr {
    +{
       key    => $_->{id},
       value  => $_,
       weight => $_->{weihgt},
    }
} @rows;
```


分かりにくいと思うので、拡大！！
コレ！！！！！

```perl
weight => $_->{weihgt}, # not weight!!!
```


順としては

+ $_->{weihgt} が undefined
+ Data::WeightedRoundRobin の、DEFAULT_WEIGHT 100が適用される
+ orz

テストコードで気づくと思いつつ、意地悪なテストじゃなかったら、すり抜けるなーと


普段、次のような恩恵を自然と預かっているんだなと改め思う
- $_->weight に対するアクセスが普段メソッド通しているので、typoに気づく
- hashへのアクセスもバリデータを通しているので、気づく
- readonly にしていればとか、、、


だから、どうしよう...
と思ったが少し閃いた

どうせ、ラップして使っているなら、default値は使わないぜ作戦

```perl
sub list2dwr (&@) {## no critic
    my $code = shift;
    return Data::WeightedRoundRobin->new([ map { $code->($_) } @_ ], { default_weight => sub { die } } )
}
```

Data::WeightedRoundRobin で、default_weight がコードリファレンスだったら実行してもらう必要があるけど、、

あとは、suppress_object_creation でreadonlyにする作戦？！

