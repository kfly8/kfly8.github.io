---
author: "kfly8"
date: "2016-01-03T00:53:00+09:00"
linktitle: "HUGO MEMO"
title: "HUGO MEMO"
tags: ["HUGO"]
description: |
  はてなブログからHUGO + Github Pages にした。
  最低限のメモを以下に記す。
---

はてなブログからHUGO + Github Pages にした。
最低限のメモを以下に記す。

<a href="https://gohugo.io/">![HUGO](/img/post/2016/hugo-logo.png)</a>

# 導入記事

- http://deeeet.com/writing/2014/12/25/hugo/
- https://blog.riywo.com/2015/09/migrate-to-huge-and-s3/
- etc

# commands

```shell
$ brew install hugo
$ hugo server -w --bind=0.0.0.0
```

# [themes](http://themes.gohugo.io/)

このブログは、[steam](http://themes.gohugo.io/steam/) を元にスマホで見やすいようにデザイン調整したつもり。

# [shortcodes](https://gohugo.io/extras/shortcodes/)

コードスニペットを簡単に呼び出しできる。

例えば、はてなブログの埋め込みをしたい場合、
以下のようなコードスニペットを用意してあげれば、
記事内で呼び出すと勝手に展開をしてくれる。

```html
#./layouts/shortcodes/hatenablog.html
<iframe src="{{ .Get 0 }}"
 class="embed-card embed-blogcard"
 scrolling="no"
 frameborder="0"
 style="display: block; width: 100%; height: 190px; max-width: 500px; margin: 10px 0px;"
></iframe>
```

```
{\{% hatenablog "//techblog.karupas.org/embed/20111117/1321542949" %\}}
```

{{% hatenablog "//techblog.karupas.org/embed/20111117/1321542949" %}}

こういうのを自前で書くのは楽しいけど、はてなは便利だったなーと思う。
embed に関しては、私の場合は、そこまで embed する機会がないので、様子見。
また、[embed.js](http://riteshkr.com/embed.js/) なるもので、統一的なI/Fで embed できるようなので、試してみてもいいかもしれない。


