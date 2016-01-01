---
author: "kfly8"
date:   "2014-09-24 00:52:11+09:00"
linktitle: "slides.comを使ってみた"
title: "slides.comを使ってみた"
tags: []
description: |
  [App::revealup](http://search.cpan.org/~yusukebe/App-revealup-0.08/lib/App/revealup.pm)で作成したslideを公開する時、
  今までgithub pagesを使っていたのですが、
  [reveal.js](https://github.com/hakimel/reveal.js/#online-editor)で紹介されているslides.comを使ってみました。
---

[App::revealup](http://search.cpan.org/~yusukebe/App-revealup-0.08/lib/App/revealup.pm)で作成したslideを公開する時、
今までgithub.ioを使っていたのですが、
[reveal.js](https://github.com/hakimel/reveal.js/#online-editor)で紹介されている[slides.com](http://slides.com/)を使ってみました。

# 結論
markdownを作ってスライド作るワークフローはほとんど（後述）変えずに
簡単にスライドが用意できて、さらに以下のようなメリットがあったので、
個人的にはもっとslides.com 使いたいなーと思いました！


（スライドの内容はともかく）試しに作成した結果
<iframe src="//slides.com/kfly8/isucon_ttp/embed?style=light" width="576" height="420" scrolling="no" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

元々はこんな感じ。->
[slide](http://kfly8.github.io/doc/2014-09-17_gotandapm/) /
[github](https://github.com/kfly8/doc/tree/gh-pages/2014-09-17_gotandapm)


# メリット

* [embedらくちんにできる！](http://help.slides.com/knowledgebase/articles/254996-embedding-decks)
* プレゼン機能が充実してる！
  * [スピーカーノート用意できて、スピーカー用のビューが用意されてる！](http://help.slides.com/knowledgebase/articles/333923)
  * [リモートコントロールできる！](http://help.slides.com/knowledgebase/articles/333925-remote-control)
  * [live streaming 機能ある！](http://help.slides.com/knowledgebase/articles/333924-live-streaming) かっこいい！
* Social 機能ある！
  * twitterとかのsocial mediaボタン！
  * [コメント機能！](http://help.slides.com/knowledgebase/articles/255205-comments)
* スライドが、HTMLなのでモバイル端末で見た時に見やすい（見やすくできる）
  * [やろうと思えばhtmlをだいたい直接修正できる](http://help.slides.com/knowledgebase/articles/405697-custom-html)
* スライドを公開した後に修正できる（良くも悪くも）
* revealjsの機能をより最大限に活かしやすい
  * iframeでビデオを再生したりできる。
* exportされるhtmlが、1 file
  * (revealjsとかの読み込みとかlibファイルのこと考えなくていい）
  * 有料オプションでpdfへのexportやDropboxへのsyncなどもできる
  * さらに、[sync先にgithubにするようなissueもあがっている](http://help.slides.com/forums/175819-general/suggestions/3173469-github-integration)ようなので、期待age



[ヘルプ一覧](http://help.slides.com/knowledgebase)にはもっと他の機能についての紹介が
簡潔に書いてあって、わかりやすい！



# やること

markdownで作ったスライドを、slides.comにあげる為にやることは、
大きく一つだけ。

markdownから適当なHTMLファイルを用意してimportするだけ。
importについては[コチラ](http://help.slides.com/knowledgebase/articles/271213-import-from-reveal-js)を参照

## importするHTMLの用意

* App::revealupで立ち上げたスライド(0.0.0.0:5000を想定)を、Chrome経由で保存（Cmd+S）((外部ファイルのmarkdownをhtml tagに展開した物が欲しいのでこんなことやってる。展開済みのhtmlを用意できればなんでもいい。展開するときに、画像パスとdata-markdown属性だけ気をつける))
* 画像のパスを絶対パスに変えてあげる
* data-markdown="" 属性を消す

雑だけど、つまりこんな感じ

```
Cmd+S
vim 0.0.0.0.html
:%s;./0.0.0.0_files;https://dl.dropboxusercontent.com/u/XXX/img/YYY;gc
:%s;data-markdown="";;g
:cat % | pbcopy
# -> slides.comの編集画面からimport
```

これを自動化できればもっと楽そう。((slides.comがmarkdownのimportに対応すればもっと楽ではある、、))

# まとめ

slides.comを使えば、markdownでスライド作るワークフローは殆ど変えずに、
プレゼン面、ソーシャルメディアの面で多くのメリットが得られて、幸せ！


