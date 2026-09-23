---
title: "VersaPro Eシリーズ タイプVRのキーボードショートカットを有効にする on Linux"
date: 2026-09-24 00:00 +09:00
tags:
    - tips
    - computer
    - linux
---

VersaPro Eシリーズ タイプVRについて、Linuxで運用しているとマイクミュートキーが働かないことに気づいた。他にも以下のキーが反応しない

- Fn-F4 : `マイクのオン／オフを切り替えます。`
- Fn-F8 : `市販の外部ディスプレイを接続しているときに、画面を表示する方法を切り換えます。`
- Fn-F10 : `機内モードのオン／オフを切り替えます。`
- Fn-F11 :  `ECOモードを切り替えます。`
- Fn-Space : `タッチパッドのオン／オフ`

`libinput debug-events`だとか`evtest`だとか`wev`だとかを駆使して、色々調査した結果、おなじみの`hwdb`をいじって解決するのが良いということになった。それでも以下のキーは使えそうにない。

- Fn-Spaceはなんか複雑なキーの組み合わせ（Ctrl-Super-無変換？）を送信するようになっているため使えない。
- Fn-F8は左Metaのコードになってしまうため使えない。
- Fn-F10はそもそもイベント自体を検出してくれないので使えない。

Fn-F4とFn-F11の両方がキーストローク全体で一つのイベントしか発生させないので`!`をつける必要がある。あとhwdbで素直にmicmuteキーに割り当ててしまうとなんかうまく動かないらしいのでF20に割り当てる。

```
sudo tee /etc/udev/hwdb.d/70-nec-versapro-e-vr.hwdb << 'EOF'
evdev:atkbd:dmi:bvn*:bvr*:bd*:svnNEC:pnPC-VEE11R5GL5LM:pvr*
 KEYBOARD_KEY_d7=!f20
 KEYBOARD_KEY_97=!battery
EOF
```

`sudo systemd-hwdb update && sudo udevadm trigger --sysname-match="event*"`しただけでは、Linuxシステムとしてはキーを認識しても、GNOMEがそれを反映しないらしいので、上のコマンドの後、**ログアウトしても問題ない状態にしてから**`sudo systemctl restart gdm`するか、あるいは素直にシステムを再起動するかすると、2つのキーが機能するようになる。

ただし、バッテリボタンについては、ただバッテリの残量を表示するだけなので、そんなに実用性がないのも事実。Windowsと同じように電源モードを切り替えさせるようにしたい。

シェルスクリプトはLLMに書いてもらった。これを`~/bin`なり`~/.local/bin`なりに保存して、`chmod +x`する。

```sh
#!/bin/sh
#  performance -> balanced -> power-saver->...
cur=$(powerprofilesctl get)
case "$cur" in
  power-saver) order="performance balanced power-saver" ;;
  balanced)    order="power-saver performance balanced" ;;
  *)           order="balanced power-saver performance" ;;
esac

for p in $order; do
  powerprofilesctl set "$p" 2>/dev/null && break
done

new=$(powerprofilesctl get)
case "$new" in
  power-saver) label="Power Saver" ;;
  balanced)    label="Balance" ;;
  performance) label="Performance" ;;
  *)           label="$new" ;;
esac

idf="${XDG_RUNTIME_DIR:-/tmp}/eco-toggle.nid"
id=$(cat "$idf" 2>/dev/null)

gdbus call --session \
  --dest org.freedesktop.Notifications \
  --object-path /org/freedesktop/Notifications \
  --method org.freedesktop.Notifications.Notify \
  -- \
  "Power Mode" "${id:-0}" "power-profile-${new}-symbolic" \
  "Power Mode" "$label" "[]" "{'transient': <true>}" -1 \
  | sed -E 's/.*uint32 ([0-9]+).*/\1/' > "$idf"
```

次に、組み込みのバッテリ残量表示設定を解除する。

```sh
gsettings set org.gnome.settings-daemon.plugins.media-keys battery-status-static "[]"
```

最後に、GNOMEの"Settings"-"Keyboard"-"View and Customize Shortcuts"-"Custom Shortcuts"でBatteryキーを押したときのコマンドとして設定すれば良い。
