---
title: "Pipewire / WirePlumber環境でヘッドフォンを外したときに再生を止めない設定"
date: 2026-01-10 18:30 +09:00
tags: 
    - computer
    - linux
    - tips
---

## はじめに

[ドキュメント](https://pipewire.pages.freedesktop.org/wireplumber/daemon/configuration/settings.html)を読め。

## 起きたこと

Fedora KDE Plasma Desktop 43環境において、ヘッドフォンを使用してGoogle ChromeやFirefoxで音楽や動画を再生中に、ヘッドフォンを取り外すと再生が停止する。

## 理由

Wireplumberが[MPRIS](https://wiki.archlinux.org/title/MPRIS)を通じてSinkがなくなると再生を一次停止する設定がデフォルトになっているから。

> linking.pause-playback
> When an audio sink is removed, pause media players that have streams playing to it. Pausing is done via MPRIS interface.
> Default value:
>       true

## 実際にやること

```bash
mkdir -p ~/.config/wireplumber/wireplumber.conf.d/
vim ~/.config/wireplumber/wireplumber.conf.d/50-no-pause.conf
```

設定ファイルの内容は以下の通り。

```conf
wireplumber.settings = {
  linking.pause-playback = false
}
```

端末から`wpctl settings --save linking.pause-playback false`でもいいらしいけど試してはいません。

## おわりに

[Claude Opus 4.5](https://claude.ai)が全部やってくれました。
