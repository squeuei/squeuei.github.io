---
title:  "Linuxでキーボードをリマップする"
date: 2024-05-27 19:00 +09:00
last_modified_at: 2026-02-19 20:00 +09:00
tags:
    - keyboard
    - tips
    - computer
    - linux
---

## Update

[システムとしての設定をいじらずにユーザ単位で設定する方法の記事]({% link _posts/2026-02-19-key-remap-revisited.md %})を書いた。

## TL;DR

以下の記事がやっていることの焼き直し。

[Linux で X よりも低レイヤでキーマップを変更する](https://zenn.dev/tmtms/articles/202109-remap-key)

## 本文

US配列キーボードへ換装したThinkpad X390にDebianをインストールして使っている。JIS配列とは異なり、主要なキーがすべて同じサイズのキートップなので使いやすい。一方で私は最近[JISキーボードを用いて変換/無変換でIMEを切り替える]({% link  _posts/2024-03-12-use-a-jis-keyboard-as-us-layout.md %})設定を推進している。コマンドを打ったり、Web検索する程度であれば問題は感じないが、長文を打つ際には、この切り替えが使えるほうがありがたい。そこで外付けキーボードの出番であるが、今までLinuxにおけるキーボードリマップに関する知見を蓄積してこなかったため、今回その技術開発を行った。

とはいえ、上のzennの内容をなぞるだけである。

xinputとevtestを使えるようにした上で`/etc/udev/hwdb.d`にファイルを作れば良い。左辺の値はevtestを用いて調べる。右辺の値は`/usr/include/linux/input-event-codes.h`を参照して決定する。

```hwdb
evdev:name:Chicony PFU-68 USB Keyboard:dmi:bvn*:bvr*:bd*:svn*:pn*
  KEYBOARD_KEY_70035=leftalt
  KEYBOARD_KEY_700E2=leftmeta
  KEYBOARD_KEY_70089=grave
  KEYBOARD_KEY_70087=capslock
```

この設定では、HHKB Lite2 JIS配列のキーボードを以下の通りリマップしている。

- 全角半角 -> 左Alt
- 左Alt -> 左Super
- ￥ -> `（グレーヴ）
- ろ -> CAPSLOCK

基本的には私がMacbookで行っているのと同じものだ。

ちなみに、上の記事で解説されている通りに、いちいち再接続しなくても以下のコマンドでキーボード設定の再読込が可能だ。

```terminal
% sudo udevadm hwdb --update
% sudo udevadm trigger
```
