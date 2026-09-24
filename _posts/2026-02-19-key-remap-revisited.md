---
title:  "kanataを使ってキーリマップする on Linux"
date: 2026-02-19 20:00 +09:00
last_modified_at: 2026-09-25 07:00 +09:00
tags:
    - keyboard
    - tips
    - computer
    - linux
---

**Revised at 2026-09-25 : userを`input`グループにいれるのは危険らしいので、ユーザごとにリマップするのを諦めて高機能リマッパとしての`kanata`を導入する記事として再構成する。**

[以前書いた記事]({% link _posts/2024-05-27-keyboard-remap-on-linux.md %})では`udev`で`hwdb`を作ってキーをリマップしたけど、今回は[jtroo/kanata](https://github.com/jtroo/kanata "jtroo/kanata: Improve keyboard comfort and usability with advanced customization" )を使ってリマップする。

## 使うもの

[jtroo/kanata: Improve keyboard comfort and usability with advanced customization](https://github.com/jtroo/kanata)

自分でビルドする場合は`rustup`でRustのツールチェーンを整備する。

## セットアップ

[Systemd unit for kanata [Linux] · jtroo/kanata · Discussion #130](https://github.com/jtroo/kanata/discussions/130#discussioncomment-11377658)を参考に。

### kanata

Githubの[Releasesページ](https://github.com/jtroo/kanata/releases)からバイナリを取ってくるか、`cargo install kanata`で自前ビルドしたのを`/usr/local/bin`あたりに置く。

### config (kanata)

私の使用例として、JIS配列のキーボードをUS配列として使用する際に用いている、CapsLockを「ろ」に割り当てる設定を示す。以下を`/etc/kanata/01-default.kbd`として保存する。

```
(defsrc
  caps  IntlRo
)

(deflayer default
  lctl  caps
)
```

### config (systemd)

上記リンクのYvan-Massonによる設定に基づいて、`systemd`のサービスとしてkanataを自動起動する。
コマンド実行が終わったら再起動。

```sh
# Based on Yvan-Masson's configuration.
# https://github.com/jtroo/kanata/discussions/130#discussioncomment-11377658
# This codeblock is not licensed under CC BY 4.0 because it's not my own creation (just in case).
sudo groupadd  --system uinput
sudo echo 'KERNEL=="uinput", MODE="0660", GROUP="uinput", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/50-kanata.rules > /dev/null
sudo useradd --no-create-home --groups input,uinput --shell /bin/false --user-group kanata
# put the binary to /usr/local/bin as root and then:
sudo chown root:kanata /usr/local/bin/kanata
sudo chmod 754 /usr/local/bin/kanata
sudo echo "[Unit]
Description=Kanata keyboard remapper
Documentation=https://github.com/jtroo/kanata
Wants=modprobe@uinput.service
After=modprobe@uinput.service

[Service]
Type=simple
User=kanata
ExecStart=/usr/local/bin/kanata --quiet --cfg /etc/kanata/01-default.kbd
Restart=no
# Security
CapabilityBoundingSet=
DeviceAllow=/dev/uinput rw
DeviceAllow=char-input
DeviceAllow=/dev/stdin
DevicePolicy=strict
PrivateDevices=true
BindPaths=/dev/uinput
BindReadOnlyPaths=/dev/stdin
BindReadOnlyPaths=/dev/input/
InaccessiblePaths=/dev/shm
LockPersonality=true
NoNewPrivileges=true
PrivateTmp=true
PrivateNetwork=true
PrivateUsers=true
# The following can not be enabled, otherwise Kanata can not open /dev/uinput.
# More hardening would require to explicitly list allowed system calls.
#ProtectClock=true
ProtectHome=true
ProtectHostname=true
ProtectKernelTunables=true
ProtectKernelModules=true
ProtectKernelLogs=true
ProtectSystem=strict
ProtectControlGroups=true
# Allow only on AddressFamily and then deny it to effectively deny everything
RestrictAddressFamilies=AF_AX25
RestrictAddressFamilies=~AF_AX25
RestrictNamespaces=true
SystemCallArchitectures=native
SystemCallErrorNumber=EPERM
SystemCallFilter=@system-service
SystemCallFilter=~@privileged
SystemCallFilter=~@resources
RemoveIPC=true
IPAddressDeny=any
RestrictSUIDSGID=true
RestrictRealtime=true
MemoryDenyWriteExecute=true
UMask=0077
[Install]
WantedBy=multi-user.target" | sudo tee /etc/systemd/system/kanata.service > /dev/null
sudo systemctl daemon-reload
sudo systemctl enable kanata.service
```

上記のコードブロックは私自身が作ったものじゃないからCC BY 4.0から除外します（この場合どういうライセンスにするのが適切なんだ？　設定ファイルは著作物じゃないからそもそも著作権が適用されない？）
