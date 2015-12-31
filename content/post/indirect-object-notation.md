---
author: "kfly8"
date: "2014-12-17T12:54:09+09:00"
linktitle: "間接オブジェクト記法のアンチパターン"
title: "間接オブジェクト記法のアンチパターン"
tags: ["perl"]
description: |
  この記事は Perl Advent Calendar 2014 の 17日目 の記事です。
---

この記事は [Perl Advent Calendar 2014](http://qiita.com/advent-calendar/2014/perl) の 17日目 の記事です。

16日目の記事は [magnolia_k_](http://qiita.com/magnolia_k_) さんの [perlの関数を第一級オブジェクトとして扱う話](http://qiita.com/magnolia_k_/items/b0a14e29c8090568f5b3) でした

# 目次

* [はじめに](#section1)
* [間接オブジェクト記法ってそもそも何？](#section2)
* [間接オブジェクト記法が活きる例](#section3)
* [ハマる例1 Try::Tiny のuse 漏れ](#section4)
* [余談 code block](#section5)
* [ハマる例2? UNIVERSAL](#section6)
* [ハマる例3 テストにて](#section7)
* [まとめ](#section8)
* [明日](#section9)


# <a name="section1">はじめに</a>

15日目の記事の id:akihiro0228 さんの記事を読んで、
[http://akihiro0228.hatenablog.com/entry/2014/12/15/221806:embed]

自分がよくハマってしまう[間接オブジェクト記法](http://perldoc.jp/docs/perl/5.8.8/perlobj.pod#Indirect32Object32Syntax)について紹介したいと思います。
<b>便乗です！</b>

# <a name="section2">間接オブジェクト記法ってそもそも何？</a>

メソッドを呼び出しの別表現です

```perl
$obj->method($a, $b); # 普段の呼び出し

method $obj $a, $b;   # 間接オブジェクト記法
```

一番なじみやすい例はnewだと思います
```perl
my $dog = new Dog;
```

普段は、誤読につながりやすいのであまり使うことをおすすめしないです
ですが、意図せず間接オブジェクト記法な呼び出しにしてしまったり、
この表現の方が簡潔に書ける例もあったりするので紹介してみたいと思います



# <a name="section3">間接オブジェクト記法が活きる例</a>

定数を扱う時に便利なconstantプラグマを使ってみます
普段使う時は、以下のような感じで使うと思います

```perl
use constant FOO => 'foo';
```

これを動的に定数定義してみたいとします

動的に定義したい場合は
`use XXX` は、`BEGIN { require XXX; do { XXX->import } }` のsugarなので、
このsugarをほどいて１つ１つ自分で順に追ってやれば変数だったりが使えます

```perl
package XXX;

BEGIN {
    require constant;
    constant->import(FOO => 'foo' x 4);
}

say FOO; # => foofoofoofoo
```

間接オブジェクト記法を使ってみると以下のようになります

```perl
package YYY;

BEGIN {
    require constant;
    import constant FOO => 'foo' x 4;
}

say FOO; # => foofoofoofoo
```

どうですかね？
普段のconstantプラグマの使い方に近く、簡潔に書けているように思います

---

逆に今度は、よくあるハマる例を書いてみます

# <a name="section4">ハマる例1 Try::Tiny のuse 漏れ</a>

ずばり、 id:karupanerura 先生の話を例を取ってみます。

[http://techblog.karupas.org/entry/20111117/1321542949:embed]


```perl
use strict;
use warnings;
use utf8;
use feature 'say';

# XXX わざとuseしないで、errorを起こす
#use Try::Tiny;

try {
    say 'foo';
    say 'bar';
}
# =>
# foo
# bar
# Can't locate object method "try" via package "1" (perhaps you forgot to load "1"?) at try2.pl line 6.
```

このエラーだとよくわからないので、
7日目の id:papix さんと11日目の id:hisaichi5518 さんが紹介しているB::Deparseを使います

[http://papix.hatenablog.com/entry/2014/12/07/150335:embed]

[http://hisaichi5518.hatenablog.jp/entry/2014/12/11/222358:embed]


```perl
# perl -MO=Deparse %

use utf8;
use warnings;
use strict;
use feature 'say';
do {
    say 'foo';
    say 'bar'
}->try;
```

do の最後に評価された 1 を package として、1->try みたいなことをしています

```
# Can't locate object method "try" via package "1" (perhaps you forgot to load "1"?) at try2.pl line 6.
```

`package`が1だとちょっと分かりにくいと思うので、
次の例を見てみてください

```perl
use strict;
use warnings;
use utf8;
use feature 'say';

#use Try::Tiny;

try {
    say 'foo';
    bless {}, 'foo'; # XXX obj を最後においてやる
}
# =>
# foo
# Can't locate object method "try" via package "foo" at try2.pl line 6.
```

Deparse すると以下のようになります

```perl
use utf8;
use warnings;
use strict;
use feature 'say';
do {
    say 'foo';
    bless {}, 'foo'
}->try;
```

今度のエラーと、Deparse した例を見ると、foo->try と呼び出しされたことが分かってもらえると思います
```
# Can't locate object method "try" via package "foo" at try2.pl line 6.
```

最後にちゃんと `use Try::Tiny` した例です

```perl
use strict;
use warnings;
use utf8;
use feature 'say';

use Try::Tiny;

try {
    say 'foo';
    bless {}, 'foo';
}
# => foo
```

```perl
# perl -MO=Deparse %

use utf8;
use Try::Tiny;
use warnings;
use strict;
use feature 'say';
try sub {
    say 'foo';
    bless {}, 'foo';
}
;
```

Deparse したコードを見ると、try に coderef が渡されていることが分かると思います


# <a name="section5">余談 code block</a>

[Try::Tiny の try のコード](https://metacpan.org/source/DOY/Try-Tiny-0.22/lib/Try/Tiny.pm#L24)
を見ると、次のようにサブルーチン名に`(&;@)`といった指定がされています

```perl
sub try (&;@) {
```

これはプロトタイプと呼ばれるものです
> http://perldoc.jp/docs/perl/5.6.1/perlsub.pod#Prototypes

これにより、blockで、coderef を表現できるので、より簡潔な記法ができます
try 以外にも、grep, mapなどが身近な例だと思います

### coderef を実行しながら、sumを取る関数を実装してみます

まずは、prototypeを使わない例

```perl
use strict;
use warnings;
use utf8;
use Test::More;
use Test::Exception;
use Test::Perl::Critic;
use List::Util qw/sum/;

sub sum_map {
    my $cb = shift;
    sum map { $cb->($_); } @_;
}

subtest 'basic' => sub {

    is sum_map(sub { $_[0] * 10 }, 1..10), 550;
};

subtest 'dies' => sub {

    dies_ok { sum_map { $_ * 10 }, 1..10; }

    # =>
    # Use of uninitialized value $_ in multiplication (*) at proto.pl line 23.
    # Odd number of elements in anonymous hash at proto.pl line 23.
    # Not a CODE reference at proto.pl line 10.
};

subtest 'critic' => sub {
    critic_ok($0);
};

done_testing;
```

次にprototypeを指定してみます

```perl
use warnings;
use utf8;
use Test::More;
use Test::Exception;
use Test::Perl::Critic;
use List::Util qw/sum/;

sub sum_map (&@) {
    my $cb = shift;
    sum map { $cb->($_) } @_;
}

subtest 'basic' => sub {

    my $ret = sum_map { $_ * 10 } 1..10;
    is $ret, 550;
};

subtest 'other use case' => sub {

    my $ret = sum_map(sub { $_[0] * 10 }, 1..10);
    is $ret, 550;
};

subtest 'critic' => sub {
    critic_ok($0); ## failed!!

    # not ok 1 - Test::Perl::Critic for "proto2.pl"
    # Failed test 'Test::Perl::Critic for "proto2.pl"'
    #   at proto2.pl line 27.
    #
    # Perl::Critic found these violations in "proto2.pl":
    # Subroutine prototypes used at line 9, column 1.  See page 194 of PBP.  (Severity: 5)
};

done_testing;
```

critic のテストが通りませんでした
blockが coderef になっているのは、確かに副作用が強いです
その為、多用はしたくないです

以下のように ## no critic と注釈をつけてあげて、
副作用を許容してあげることができます
> http://search.cpan.org/~thaljef/Perl-Critic-1.123/lib/Perl/Critic.pm#BENDING_THE_RULES

```perl
use strict;
use warnings;
use utf8;
use Test::More;
use Test::Exception;
use Test::Perl::Critic;
use List::Util qw/sum/;

sub sum_map (&@) { ## no critic
    my $cb = shift;
    sum map { $cb->($_) } @_;
}

subtest 'critic' => sub {
    critic_ok($0), '注釈をつけることで回避';
};

done_testing;
```

# <a name="section6">ハマる例2? UNIVERSAL</a>

間接オブジェクトわざとらしく使ってみたいと思います
`can`を使っているのがわざとらしいです！

```perl
use strict;
use warnings;
use utf8;
use feature qw/say/;

my $dog = bless {}, 'Dog';

if (can $dog 'bow') {
    say 'can bow';
}
else {
    say 'cannot bow';
} # => cannot bow

```
```perl
#perl -MO=Deparse %

use utf8;
use warnings;
use strict;
use feature 'say';
my $dog = bless({}, 'Dog');
if ($dog->can('bow')) {
    say 'can bow';
}
else {
    say 'cannot bow';
}
```


```perl
# original
if (can $dog 'bow')

# Deparse
if ($dog->can('bow')) {
```

$dogにcan methodは生やしていないですが、
Perlの場合、全てのクラスは暗黙的にUNIVERSAL を元にするようになっているので、
$dog->can で UNIVERSAL->can が呼び出しされます


試しに、UNIVERSALのcanが呼び出しされないように、
このパッケージに `can` というサブルーチンを生やしてみます

```perl
use strict;
use warnings;
use utf8;
use feature qw/say/;

my $dog = bless {}, 'Dog';

sub can { 1 }

if (can $dog 'bow') { # XXX => syntax error
    say 'can bow';
}
else {
    say 'cannot bow';
}

=pod

syntax error at can2.pl line 10, near "$dog 'bow'"
can2.pl had compilation errors.
use utf8;
use warnings;
use strict;
use feature 'say';
my $dog = bless({}, 'Dog');
sub can {
    use warnings;
    use strict;
    use feature 'say';
    1;
}
```

$dog と 'bow' の間にカンマがなくて、エラーになりました
もちろんカンマを入れれば、次のように、'can bow'と表示されます


このカンマのあるなしの意思表示で、間接記法であるかどうか示します
それは意思表示としては、わかりずらいので、誤読の元になりやすいです


```perl
use strict;
use warnings;
use utf8;
use feature qw/say/;

my $dog = bless {}, 'Dog';

sub can { 1 }

if (can $dog, 'bow') { # リストにするため、カンマを入れた
    say 'can bow';
}
else {
    say 'cannot bow';
} # => can bow

```


このように暗黙に定義されたメソッドで、間接オブジェクト記法が出てくるなんて、
あまりないと思いますが、良い機会なので、
UNIVERSALに生えているメソッドを洗ってみます


Class::Inspector を使うと、`can, isa, VERSION, DOES` ということが分かります
http://perldoc.perl.org/UNIVERSAL.html の通りです
これで、もう多分、if (isa $foo 'FOO') とやってハマること？もないでしょう？


```perl
use strict;
use warnings;
use utf8;

package Dog;

sub new { bless {}, shift };

package main;

use Test::More;

my $dog = Dog->new;

subtest 'isa' => sub {
    isa_ok $dog, 'Dog';
    isa_ok $dog, 'UNIVERSAL';
};

subtest 'methods' => sub {

    ok $dog->can('can');
    ok $dog->can('isa');
    ok $dog->can('VERSION');
    ok $dog->can('DOES');
};

subtest 'methods count' => sub {

    require Class::Inspector;
    is scalar @{Class::Inspector->methods('UNIVERSAL')}, 4;

    is_deeply(Class::Inspector->methods('Dog'), [qw/new/]);
};

done_testing;
```

# <a name="section7">ハマる例3 テストにて</a>

生々しいですが、例2のClass::Inspector を使っていて、今、ミスった例です。

```perl
use strict;
use warnings;
use utf8;

package Dog;

sub new { bless {}, shift };

package main;
use Test::More;
use Class::Inspector;

is_deeply Class::Inspector->methods('Dog'), [qw/new/];
# XXX failed!!
# => Can't locate object method "is_deeply" via package "Class::Inspector" at can5.pl line 12

=pod
use utf8;
package Dog;
sub new {
    use warnings;
    use strict;
    bless {}, shift();
}
package main;
sub BEGIN {
    use warnings;
    use strict;
    require Test::More;
    do {
        'Test::More'->import
    };
}
use Class::Inspector;
use warnings;
use strict;
'Class::Inspector'->is_deeply->methods('Dog'), ['new'];

```

これは、要点だけを絞り出すと次のようになっています

```perl
package Foo;
sub new { bless {}, shift() }

use strict;
use warnings;
use utf8;

use Test::More;

isa_ok Foo->new, 'Foo';

done_testing;
```

```perl
# Deparse

package Foo;
sub new {
    bless {}, shift();
}
use utf8;
use Test::More;
use warnings;
use strict;
'Foo'->isa_ok->new, '???';
done_testing();
/Users/kfly8/aa.pl syntax OK
```

実際にはまった例では、
```perl
is_deeply Class::Inspector->methods('Dog'), [qw/new/];
```
が、次のように評価されてしまい、残念なことになりました
```perl
'Class::Inspector'->is_deeply->methods('Dog'), ['new'];
```

括弧付きで呼び出してあげてください
```perl
is_deeply(Class::Inspector->methods('Dog'), ['new']);
```


# <a name="section8">まとめ</a>

* Perl には、method を呼び出す方法に、method $obj LIST のような呼び出し方もあり、それを間接オブジェクト記法という
* 使いどころを（すごい）選べば、import constant FOO => 'foo' のような簡潔な表現になる
* よくあるハマり方
    * 関数のexport漏れ
        * 余談 prototype
    * 暗黙なmethod定義
    * func Class->foo の Class->func->foo という誤読しやすい評価順


## 補足 この記事のコードは、gistでも見れますー。
[https://gist.github.com/kfly8/254fd149f7a07924ef63#file-constant-pl:title]



# <a name="section9">明日</a>

18日目は、[wanji](http://qiita.com/waniji) さんです！



