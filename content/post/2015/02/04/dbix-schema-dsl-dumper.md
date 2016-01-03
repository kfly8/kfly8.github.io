---
author: "kfly8"
date: "2015-02-04T21:00:12+09:00"
linktitle: "DBIx::Schema::DSL::Dumper というのを書きました"
title: "DBIx::Schema::DSL::Dumper というのを書きました"
tags: ["perl"]
description: |
  songmuさんの書いた DBIx::Schema::DSL を使いたい状況が出て来て、
  すでに結構もりもり書かれてたDDLを移行する為に書いてみました。
---

songmuさんの書いた[DBIx::Schema::DSL](https://github.com/Songmu/p5-DBIx-Schema-DSL)
を使いたい状況が出て来て、
すでに結構もりもり書かれてたDDLを移行する為に書いてみました。

使い方は、[Teng::Schema::Dumper](http://search.cpan.org/~satoh/Teng-0.19/lib/Teng/Schema/Dumper.pm)
と同じように、`$dbh` を渡すだけで、DSLが吐き出せます。

雰囲気は次のような感じです！

```perl
use DBI;
use DBIx::Schema::DSL::Dumper;

my $dbh = DBI->connect('dbi:mysql:dbname=test', 'root', '');
print DBIx::Schema::DSL::Dumper->dump(
    dbh => $dbh,
    pkg => 'Foo::DSL',
);

```


よかったら使ってほしいです！


