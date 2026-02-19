---
title:  "ユーザごとにキーリマップする on Linux"
date: 2026-02-19 20:00 +09:00
tags:
    - keyboard
    - tips
    - computer
    - linux
---

[以前書いた記事]({% link _posts/2024-05-27-keyboard-remap-on-linux.md %})では、システム全体でのキーリマップ設定をしているけど、今回はシステム全体を汚染せずユーザごとに設定する方法を記す。

## 使うもの

[jtroo/kanata: Improve keyboard comfort and usability with advanced customization](https://github.com/jtroo/kanata)

自分でビルドする場合は`rustup`でRustのツールチェーンを整備する。

## セットアップ

### kanata

Githubの[Releasesページ](https://github.com/jtroo/kanata/releases)からバイナリを取ってきてパスを通すか、`cargo install kanata`で自前ビルドする。

### uinput

[uinput](https://docs.kernel.org/input/uinput.html)カーネルモジュールを使えるようにする設定を公式ドキュメントに従って行う。

[kanata/docs/setup-linux.md at main · jtroo/kanata](https://github.com/jtroo/kanata/blob/main/docs/setup-linux.md)

```sh
sudo groupadd uinput
sudo usermod -aG input $USER
sudo usermod -aG uinput $USER
sudo tee /etc/udev/rules.d/99-input.rules > /dev/null <<EOF
KERNEL=="uinput", MODE="0660", GROUP="uinput", OPTIONS+="static_node=uinput"
EOF
sudo tee /etc/modules-load.d/uinput.conf << 'EOF'
uinput
EOF
```

再起動してグループとモジュールの設定を適用する。

### config (kanata)

私の使用例として、JIS配列のキーボードをUS配列として使用する際に用いている、CapsLockを「ろ」に割り当てる設定を示す。以下を`~/.config/kanata/10-jis2us.kbd`として保存する。

```
(defsrc
  caps  IntlRo
)

(deflayer default
  lctl  caps
)
```

### config (systemd)

ユーザサービスとしてkanataを自動起動する。以下はcargoでセットアップしたこと前提。必要に応じてExecStartのパスを変更したり`kanata`のパスを通したりすること。

```sh
mkdir -p ~/.config/systemd/user/
cat > ~/.config/systemd/user/kanata.service << 'EOF'
[Unit]
Description=Kanata

[Service]
ExecStart=%h/.cargo/bin/kanata -c %h/.config/kanata/10-jis2us.kbd
Restart=on-failure
RestartSec=3

[Install]
WantedBy=default.target
EOF

systemctl --user daemon-reload
systemctl --user enable --now kanata.service
```
