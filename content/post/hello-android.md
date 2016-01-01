---
author: "kfly8"
date:   "2014-01-20T22:52:40+09:00"
linktitle: "Hello Android"
title: "Hello Android"
tags: ["android"]
description: |
  いまさら、、あんどろいどにはろーわーるどしました
---

いまさら、、あんどろいどにはろーわーるどしました（え

備忘録です。
MyFirstAppを作成するところまでは手順通りだった。それだけなのにつかれる事態になってしまった＞＜

* http://developer.android.com/training/basics/firstapp/creating-project.html
* http://developer.android.com/training/basics/firstapp/running-app.html


# ant debugでまずはまった

## まず、ant がないと怒られる。次で解決

```sh
brew update
brew install ant
```
 * http://superuser.com/questions/610157/how-do-i-install-ant-on-os-x-mavericks
 * ほんとはbrew updateも諸処とおらず、brew doctorでPATH変更したりごにょごにょした

##  build.xmlがなくて、buildできない

* android update で解決
```sh
android update project --name MyFirstApp --target android-19 --path ~/android/workspace/MyFirstApp
```
* http://stackoverflow.com/questions/5572304/building-android-sample-project-using-ant
 * targetは、MyFirstApp/local.properticesから参照した

#  apkを端末にインストールできなくてはまった

## error: device offline

* adb devicesしてもでてこない。端末外して、次をしてから、接続し直して解決。
```sh
adb kill-server
adb start-server
```
 * http://stackoverflow.com/questions/10680417/error-device-offline

## error: more than one device and emulator

* 端末を指定して解決
```sh
% adb devices
List of devices attached
emulator-5554   offline
XXX      device
% adb -s XXX  install bin/MyFirstApp-debug.apk
```
 * http://yktmnb.b.sourceforge.jp/?p=20


とりあえず、はろーあんどろいど

