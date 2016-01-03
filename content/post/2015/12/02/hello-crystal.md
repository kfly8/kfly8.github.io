---
author: "kfly8"
date: "2015-12-02T18:39:52+09:00"
linktitle: "Hello, Crystal"
title: "Hello, Crystal"
tags: ["crystal"]
description: |
  この記事は Crystal Advent Calendar 2015 の 2 日目の記事です
---

**この記事は [Crystal Advent Calendar 2015](http://www.adventar.org/calendars/800) の 2 日目の記事です**

昨日は、[pine613](https://twitter.com/pine613) さんの [これから Crystal を始める方へ: Crystal 日本語情報まとめ](http://qiita.com/pine613/items/7407e56771b504bed82f) でした。
[公式ドキュメントの日本語訳](http://ja.crystal-lang.org)が整っているの素敵ですね!!


今日は、crystal の setup から、heroku に deploy するまで、です。
完全、後発です。厳しいです。詳しいことは、↓を見ると良いのではないでしょうか!?

[deploying-a-crystal-application-to-heroku](https://subvisual.co/blog/posts/63-deploying-a-crystal-application-to-heroku)

[crystal-on-heroku](http://zephiransas.github.io/blog/2015/11/10/crystal-on-heroku/)


# setup

- *env 最高

```sh
% anyenv install crenv
% crenv install 0.9.1
% crenv rehash
% crystal -v
Crystal 0.9.1 [b3b1223] (Fri Oct 30 03:26:53 UTC 2015)
```

# initialize app

```sh
% crystal init app app # `app` という名前のapplication の雛形用意
% cd app
% git commit -m 'initial commit'
```

# create app

- crystal-lang.org の冒頭のHello返すだけのHTTP server に OptionParserでport食わせられるようにしただけ

```ruby
% cat src/app.cr

require "http/server"
require "option_parser"

port = 8080
OptionParser.parse! do |parser|
  parser.on("-p PORT", "--port=PORT", "Set server port") { |p| port = p.to_i }
end

server = HTTP::Server.new("0.0.0.0", port) do |req|
  HTTP::Response.ok "text/plain", "Hello"
end

puts "Listening on http://0.0.0.0:#{port}"
server.listen
```

- 試しに実行してみる

```
# 即時実行
% crystal run src/app.cr
Listening on http://0.0.0.0:8080

# ビルドしてあげる
% crystal build src/app.cr
% ./app -p 5000
Listening on http://0.0.0.0:5000
```

# deploy to heroku

- Procfile とりあえず用意して、手元の環境で、ps 動かしてみる

```sh
% cat Procfile
web: ./app -p $PORT
```

```sh
% heroku local
forego | starting web.1 on port 5000
web.1  | Listening on http://0.0.0.0:5000
```

```
% git add .
% git commit -m 'update'
```

## heroku create

- ↓のカスタムビルドは、適宜・・好きなのを用意してください
  - 今回は手抜きで、crystal version が、手元では、`0.9.1` だけど、`0.9.0` でビルドされるカスタムビルドパックを利用しました:p
  - 今回のカスタムビルドの要件は、`0.9.0` でのビルド。`shard.yml` があること。`./src/app.cr` がソースファイルで、 `./app` にリリースビルドされて、`Procfile` で `./app` を呼び出している感じですね。

```
% heroku create --buildpack https://github.com/scaint/heroku-buildpack-crystal.git
% git push heroku master

Counting objects: 16, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (10/10), done.
Writing objects: 100% (16/16), 306.50 KiB | 0 bytes/s, done.
Total 16 (delta 0), reused 0 (delta 0)
remote: Compressing source files... done.
remote: Building source:
remote:
remote: -----> Fetching set buildpack https://github.com/scaint/heroku-buildpack-crystal.git... done
remote: -----> Crystal app detected
remote: -----> Installing Crystal 0.9.0
remote: -----> Installing Shards 0.5.3
remote: -----> Installing Dependencies
remote: -----> Compiling src/app.cr
remote:
remote: -----> Discovering process types
remote:        Procfile declares types -> web
remote:
remote: -----> Compressing... done, 409K
remote: -----> Launching... done, v3
remote:        https://arcane-harbor-8307.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/arcane-harbor-8307.git
 * [new branch]      master -> master
```

```
% heroku open
```

# 反省

アドベントカレンダー事前に準備する

明日は、　5t111111　さんです！

