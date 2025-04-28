---
title: "Linuxデスクトップ難しすぎる（Waylandと格闘した回）"
date: 2025-04-29 01:00 +09:00
tags:
    - computers
    - linux
    - tips
---

Fedora 42 Workstation KDE Plasma Editionを使い始めたら、VSCodeで日本語入力がうまくいかない。ibusを窓から投げ捨ててfcitx5にして一件落着、と思いきや、大体は大丈夫なのだけど、キーを速く打つと時々最初の一文字が直接入力として認識されてしまう。いろいろ設定をいじくり回して、結局見つけた解決策がこれだった。WaylandにIMを任せるとうまくいかないらしい。わかるか、そんなん。

```bash
echo 'gtk-im-module=fcitx' >> $HOME/.config/gtk-3.0/settings.ini
```

[Wayland, VSCode, and Fcitx5](https://gist.github.com/squeuei/87334184966dc51946180858552bdbef)
