---
author: "kfly8"
date:   "2015-12-20T20:43:25+09:00"
linktitle: "About Dancer2(ja)"
title: "About Dancer2(ja)"
tags: ["perl", "dancer"]
description: |
  この記事は、Perl5 Advent Calendar 2015 の20日目の記事です。
  Web Application Framework の Dancer2 について紹介したいと思います。
---

この記事は、[Perl5 Advent Calendar 2015](http://qiita.com/advent-calendar/2015/perl5) の20日目の記事です。

Web Application Framework の Dancer2 について紹介したいと思います。

まずは、YAPC::Asia 2014 の LT から。
こちらは、Dancer2 のリリースマネージャーの [SawyerX](https://twitter.com/perlsawyer) による、Dancer2の超速紹介です。
Dancer2 の要点が凝縮されたLTです。

{{% youtube 0gapHdDTRUk %}}

目を凝らすとこんなスライドが見えます。DSLで、ルーティングを記述します。

```perl
package MyApp;
use Dancer2;

get '/' => sub {
   template index => {
      name => 'Sawyer X',
      event => 'YAPC::Asia'
   }
};
```

JSON を返却するようなAPIも簡単に用意できます。
```perl
package MyApp::API;
use Dancer2;

set serializer => 'JSON';

get '/' => sub {
    return {
        hello => 'world',
    }
};
```

Dancer2 は Plack化されているので、plackupできます。

```perl
# app.psgi
use MyApp;
my $app = MyApp->psgi_app;
```

```sh
plackup app.psgi
```

ここまでが、LTの抜粋です。


Dancer2 は、元々、Ruby のSinatra を Perl に port した Dancer の次バージョンです。
Dancer の DSL はほぼそのままに、設計面での調整が大幅に入っています

> http://advent.perldancer.org/2014/2

例えば、以下のような対応が行われています。

* Plack 対応
* DSL 実装を、Top 空間から、Dancer2::Core::DSL に分解
* Moo 採用
* デザインパターンの調整（デメテルの法則...）

この対応に関して、2011 年の Dancer Advent Calendar にて、（超訳ですが）「全部書き換えるぞい」という記事があります。

> http://advent.perldancer.org/2011/8

利用者が多数のプロダクトにおいて、こういった完全な書き換えは、個人的な興味をそそりました。
今現在も活発に開発され、Dancer という名前を冠するPerl [カンファレンス](https://www.perl.dance/)も開かれています。

Dancer を作った sukria のポスト。
[perl-dancer-2015-report](http://blog.sukria.net/2015/10/22/perl-dancer-2015-report)

最近の開発状況の話。

{{% youtube UbU5R-SHbDE %}}

（Dancer2::XS ってあって、すごいってなった）


もう少し、Dancer2 の特徴を触れて、記事を終わりにしたいと思います。
（と言っても、ほぼ、[2014 年の dancer advent calendar](http://advent.perldancer.org/2014) ((SawyerX の一人アドベントカレンダー。つよい。)) からの抜粋です。


> http://advent.perldancer.org/2014/5

fatpack、つまり一枚スクリプトにするのをサポートしています。
（なので、XSモジュールは、recommend 扱いです）

ポータビリティを考えると良い選択になるかもしれません。

> http://advent.perldancer.org/2014/16

コマンドラインで利用も考慮されています。

```perl
use Dancer2;

warn config->{environment}; # => development
warn dancer_version;        # => 0.165000
```

> http://advent.perldancer.org/2014/10

複数のアプリケーションの同居が考慮されています。
Dancer2 に appname を渡すことで、config やdbh などが、appname ごとに管理されます。

以下の例だと、
MyApp::Foo1, Foo2 で、一つの Dancer2::Core::App obj が作られ、
Bar にもう一つの Dancer2::Core::App obj が作られます。

```perl
package MyApp::Foo1 {
  use Dancer2 appname => 'foo';
  get '/' => sub { ... }
}

package MyApp::Foo2 {
  use Dancer2 appname => 'foo';
  get '/' => sub { ... }
}

package MyApp::Bar {
  use Dancer2;
  get '/' => sub { ... }
}
```

# まとめ

* なんだか Dancer2 楽しそうだぞ

明日は、[akihiro_0228](https://twitter.com/akihiro_0228) さんです。



