---
title: "Linuxデスクトップ難しすぎる（Waylandと格闘した回）"
date: 2025-04-29 01:00 +09:00
tags:
    - computer
    - linux
    - tips
---

Fedora 42 Workstation KDE Plasma Editionを使い始めたら、VSCodeで日本語入力がうまくいかない。ibusを窓から投げ捨ててfcitx5にして一件落着、と思いきや、大体は大丈夫なのだけど、キーを速く打つと時々最初の一文字が直接入力として認識されてしまう。いろいろ設定をいじくり回して、結局見つけた解決策がこれだった。WaylandにIMを任せるとうまくいかないらしい。わかるか、そんなん。

```bash
echo 'gtk-im-module=fcitx' >> $HOME/.config/gtk-3.0/settings.ini
```

> [Wayland, VSCode, and Fcitx5](https://gist.github.com/squeuei/87334184966dc51946180858552bdbef)
>
> ## 現象
>
> Fcitx5で速く日本語を打つと最初の一文字目が直接入力のように扱われる。
>
> ## 環境
>
> - OS: Fedora 42 Workstation KDE Plasma Desktop
> - エディタ: VSCode (公式ビルド)
> - IMEフロントエンド: Fcitx5
> - 変換エンジン: anthy-unicode
> - KDE 仮想キーボード: Fcitx5
> - `$HOME/.config/code-flags.conf`:
>   ```
>   --enable-features=UseOzonePlatform
>   --ozone-platform=wayland
>   --enable-wayland-ime
>   --wayland-text-input-version=3
>   --gtk-version=4
>   ```
>
> ## 解決方法
>
> [この記事](https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland#XMODIFIERS)に従って、GTK3がFcitxを使うよう設定する:
>
> ```
> echo 'gtk-im-module=fcitx' >> $HOME/.config/gtk-3.0/settings.ini
> ```
>
> その他の設定は[Using Fcitx 5 on Wayland - Fcitx](https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland#KDE_Plasma)を参考に。要するに、`.profile`や`.bash_profile`などの適当なファイルで`XMODIFIERS=@im=fcitx`だけ環境変数として設定する。
>
>
> ## おまけ
>
> もしKDE Plasma + Wayland + VSCode + ibusで使いたいなら`$HOME/.config/gtk-3.0/settings.ini`で `gtk-im-module=ibus`を設定する。でも変換候補を出したところでおかしくなるので結局使えてない。
 
ちなみに、あとでVMにDebianを入れて検証したら、`im-config`をpurgeして`$GTK_IM_MODULE`、`$QT_IM_MODULE`が設定されていない状態を作っても、VSCodeでちゃんと日本語が打てました。
