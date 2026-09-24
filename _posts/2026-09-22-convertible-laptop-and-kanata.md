---
title: "コンバーチブルPC(2-in-1 ノートPC)とkanata"
date: 2026-09-22 00:00 +09:00
last_modified_at: 2026-09-25 07:00 +09:00
tags:
    - tips
    - computer
    - linux
---

**Comment at 2026-09-25 : kanataをuser serviceで起動していたときに書いた記事なのでsystemctlの記述はuser serviceが前提となっている。**

[先日導入したVersaPro Eシリーズ タイプVR]({% link _posts/2026-09-20-versapro-e-series-type-vr.md %})について、タブレットモード時に音量キーが効かなくなることに気づいた。色々調べてみると、どうやら[キーリマップに用いている`kanata`]({% link _posts/2026-02-19-key-remap-revisited.md %})がタブレットモード時に全部のボタンをひとまとめで無効にしてしまっているようだ。

以下、調査と対策について。

下記のコマンドを実行すると、ボリュームボタンのデバイス名、タブレットモードへの切り替え検知、ボタンを押したことを検知した際にその内容が表示される。

```sh
sudo apt update && sudo apt install libinput-tools -y
sudo libinput debug-events
```

このとき、タブレットモードに入る前はちゃんとボリュームボタンとしてイベントが発生しているのに、タブレットモードに入ったあとはイベントが発生していない。下記のコマンドで`evtest`で個別のデバイスについて調べようとすると、何かのプロセスが掴んでるから調べられないよ、と表示される。

```sh
sudo apt install evtest -y
sudo evtest /dev/input/eventX
```

（Xにはイベントの番号が入る。ちなみに、私の場合はevent11 Intel HID 5 button arrayだった）

その掴んでるプロセスが何かを下記のコマンドで調べる。

```sh
fuser -v /dev/input/eventX
```

すると、私の環境で出てきたのは`gnome-shell`と`kanata`だった。試しに`kanata`を`systemctl --user stop kanata`で止めると、音量ボタンはタブレットモードでも動作した。

さて、どうするか。`kanata`は[`linux-dev-names-exclude`を使ってデバイス名でgrab対象から外すことができる](https://jtroo.github.io/config.html#linux-only-linux-dev-names-exclude "Kanata Configuration Guide")。これを使うことにした。キーリマップを設定しているファイルに下記の記述を追加する。デバイス名は環境に合わせて変更する。

```kanata
(defcfg
  linux-dev-names-exclude (
    "Intel HID 5 button array"
  )
)
```

`systemctl`でserviceを再起動して、ついでにどのデバイスをgrabしているかを確認する。

```sh
systemctl --user restart kanata
journalctl --user -u kanata -e
```

これで音量ボタンに関係するデバイスを掴んでいなければ一件落着ってワケ。まだ余計なものをgrabしていたら`linux-dev-names-exclude`に追加しておいてもいいかもしれない。
