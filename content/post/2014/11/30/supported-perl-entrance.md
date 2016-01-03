---
author: "kfly8"
date:   "2014-11-30T19:02:59+09:00"
linktitle: "#Perl入学式 のお手伝いさせてもらってきた"
title: "#Perl入学式 のお手伝いさせてもらってきた"
tags: ["perl"]
description: |
  Perl入学式 に参加するのは初めてだったのですが、 サポーターとして参加させてもらいました。 お疲れ様でした！特に講師のid:xtetsuji さんお疲れ様でした！! 「サブルーチンと正規表現」がお題でした。
---

[Perl入学式](http://www.perl-entrance.org/) に参加するのは初めてだったのですが、
サポーターとして参加させてもらいました。
お疲れ様でした！特に講師のid:xtetsuji さんお疲れ様でした！!


「サブルーチンと正規表現」がお題でした。
資料は[コチラ](https://github.com/perl-entrance-org/workshop-2014-04/blob/master/slide.md)

寿司はコチラ。
<blockquote class="twitter-tweet" lang="ja"><p>Perl入学寿司 <a href="https://twitter.com/hashtag/Perl%E5%85%A5%E5%AD%A6%E5%BC%8F?src=hash">#Perl入学式</a> (@ 立ち喰い寿司 都々井 in 品川区, 東京都) <a href="https://t.co/dBvh02MXCZ">https://t.co/dBvh02MXCZ</a> <a href="http://t.co/MVMMTHvfco">pic.twitter.com/MVMMTHvfco</a></p>&mdash; OGATA Tetsuji (@xtetsuji) <a href="https://twitter.com/xtetsuji/status/538527365635514368">2014, 11月 29</a></blockquote>
美味かった。


# 面白かった(?)罠がコチラ

```perl
use strict;
use warnings;
use utf8;

sub dump_hash {
    my $hash = @_;
    $hash->{foo};
}

dump_hash({foo => 1, bar => 2});
# => Can't use string ("1") as a HASH ref while "strict refs" in use at foo.pl line 7.
```

生徒:「`1`って何?!?!」
おれ:「配列をスカラーで評価して、配列長..ですね...」
生徒:「??????」
おれ:「ですよね!!!!!!!!」


幸い？他のプログラミング言語を触っている人だったからか、
「Perlだと、どう値を使いたいか意思表示できて、
例えば、配列が欲しいのか、スカラが欲しいのかなど。
逆に言うと意思表示に慣れるまで、ハマります！！」
でコンテキストのことを濁し、次のような引数の取り方の例を見てもらったら納得してもらえたよう。

```perl
sub dump_hash2 {
    my $h   = @_; # bad...
    my ($h) = @_; # ok (補足: 括弧が、優先順位の括弧に見えてしまったよう。 my ($a,$b) = @_ の例も併わせて紹介して、リストの括弧と分かってもらえた。
    my ($h,)= @_; # ok (さらに補足: 人によっては配列をより意識する為にこう書く人もいる
    my $h   = shift; # ok
    my $h   = $_[0]; # ok
}
```


蛇足にこんな例も話したら、楽しんでもらえたよう（多分）

```perl
my @a  = ('a', 'b', 'c', 'd');
#my @a = qw/ a b c d /; # これで上と同じ意味

my %hash  = ( a => 1, b => 2 );
#my %hash = ( 'a', 1, 'b', 2 );  # これで上と同じ意味
#my %hash = qw/ a 1 b 2 /;  # これも同じ意味
my @b     = ( 'a', 1, 'b', 2 );  # さらに配列として受け止めることもできる
```

慣れるのは苦労するかもしれないけど、、、
例えば、「対になっているデータが欲しいから、ハッシュにしよう！」
とか思った時に、特にこねくり回したことしなくて良いのが、個人的に楽だと思っている。

# 完全なる蛇足

楽しみついでに、[サブルーチンの定義](https://github.com/perl-entrance-org/workshop-2014-04/blob/master/slide.md#%E3%82%B5%E3%83%96%E3%83%AB%E3%83%BC%E3%83%81%E3%83%B3%E3%81%AE%E5%AE%9A%E7%BE%A9-2)に関して、
「!」「数字」や「日本語」をサブルーチン名に使ったりする応用例？を書いた。

この辺、YAPC::Asia 2014 のid:karupaneruraの[meta programmingの話](http://yapcasia.org/2014/talk/show/25f99ab0-fa15-11e3-b7e8-e4a96aeab6a4) が詳しい。(demoが多いので、動画がおすすめ）

```perl
use strict;
use warnings;
use utf8;

use Test::More;

{
    no strict qw/refs/;
    *{'world!'} = sub { 'world' };
    is *{'world!'}{CODE}->(), 'world', 'symbol table call';
}

done_testing;
```

```perl
use strict;
use warnings;
use utf8;

use Test::More;

sub こんにちは { 'hello' }
is こんにちは(), 'hello';

done_testing;
```

```perl
use strict;
use warnings;
use utf8;

use Test::More;
use Package::Stash;

my $stash = Package::Stash->new('Foo');
$stash->add_symbol('&hello world!', sub { 'hello' });

subtest 'by get_symbol' => sub {
    my $code = $stash->get_symbol('&hello world!');
    is $code->(), 'hello'
};

subtest 'by pkg' => sub {
    ok Foo->can('hello world!'), 'can?';
    my $code = Foo->can('hello world!');
    is $code->(), 'hello';

    my $method = 'hello world!';
    is Foo->$method(), 'hello';
};

done_testing;
```


そんなこんなで、Perl入学式楽しませてもらいました！！
ありがとうございました！
またの機会もよろしくお願いします！


