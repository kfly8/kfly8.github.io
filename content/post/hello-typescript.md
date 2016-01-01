---
author: "kfly8"
date:   "2013-11-16 18:18:56+09:00"
linktitle: "TypeScriptさわるぞー #TODO"
title: "TypeScriptさわるぞー #TODO"
tags: ["javascript"]
description: |
  jsを大きな規模でもちゃんと動くようにしたいと、のたまったので、
  偉い人たちが作ったaltjsを勉強させてもらいます　ref http://kfly8.hatenablog.com/entry/2013/11/16/005340
---

jsを大きな規模でもちゃんと動くようにしたいと、のたまったので、
偉い人たちが作ったaltjsを勉強させてもらいます　ref http://kfly8.hatenablog.com/entry/2013/11/16/005340

[#発火村](https://twitter.com/search?q=%23%E7%99%BA%E7%81%AB%E6%9D%91&src=hash) 中、id:gfx 先生におすすめしてもらった[TypeScript](http://www.typescriptlang.org/)を見てみます＞＜

ぱっと見: # TODO

* jsのsuperset
* 見た目はほとんど一緒
* I/Fに型を添えて分かりやすくしている

# 導入
```sh
% npm install -g typescript
% cat > hello.ts
function hello(s: string) {
  return 'hello ' + s;
}
console.log(hello('typescript'));
^D
% tsc hello.ts
% node hello.ts
```
# grunt-typescrpt
```sh
npm install grunt-typescript
https://github.com/kfly8/ts-sample
```

# TypeScriptのドキュメントのメモ

* http://www.typescriptlang.org/Content/TypeScript%20Language%20Specification.pdf
* <b>TODO あとで読みやすくして、検証する、、、ほとんどこぴぺだよ、やばいよやばいよ</b>

## Types

```javascript
// primitive type. ( number, boolean, string, void, null and undefined and user defined enum types)
function hello(str: string) {
    return 'hello' + str;
}

// function type
function hoge(callback: (result: string) => any ) {
    ...
}

hoge(function(result: string) {
    ...
});

// object type

// * object type literal ( ObjectType,ArrayType,FunctionType and ConstructorType  )
var aaa: () => {
    x: number; y: number;
};

// * object type interface // PerlのMouseTypeみたい
interface Friend {
    name: string;
    birthday?: string; # optional field
}

function add(friend: Friend) {
    var name = friend.name;
}

add({ name: 'taro' });
add({ birthday: '20130101' }); # error, name required
add({ name: 'hanako', birthday: '20130201' });


// Structial Subtypes

interface Point {
    x: number;
    y: nubmer;
}

function getX(p: Point) {
}

Class CPoint {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

getX(new CPoint(0,0)); // ok!!
getX({ x: 1, y: 2: z: 3); // ok!! Extra fields ok
getX({ x: 1 ); // ng!! supplied parameter's not Point

```

## Class && Module

```javascript
class Animal {
    constructor(public age: number) {
    }
    addAge(amount: number) {
        this.age += amount;
        return this.age;
    }
}
```

```javascript
var Animal = (function () {
    function Animal() {
        this.age = 0;
    }
    Animal.prototype.addAge = function(amount) {
        this.age += amount;
        return this.age;
    };
    return Animal;
})();
```

```javascript
module Hoge {
    var s = "hello";
    export function f() {
        return s;
    }
}

Hoge.f();
Hoge.s; // Error, s is not exported // variable 's' is private
```

```javascript
var Hoge;
(function(exports) {
    var s = "hello";
    function f() {
        return s;
    }
    exports.f = f;
})(Hoge||(Hoge={}));
```

## declare

```javascript
declare var document;
document.title = "Hello";
```

## 外部モジュール

```javascript
# File main.ts:
import log = require("log");
log.message("hello");

# File log.ts:
export function message(s: string) {
    console.log(s);
}
```

```javascript

# point.ts
export = Point;
class Point {
    constructor(public x: number, public y: number) { }
    static origin = new Point(0, 0);
}

# use
import Pt = require("point");
var p1 = new Pt(10, 20);
var p2 = Pt.origin;
```

[温泉発火村](http://atnd.org/events/43858)なう

[![](http://instagram.com/p/gw7VF5IDPl/media/?size=l)](http://instagram.com/p/gw7VF5IDPl/)

[#発火村 ！！！！！！！！！！！](http://instagram.com/p/gw7VF5IDPl/)

