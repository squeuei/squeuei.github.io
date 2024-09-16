---
title: "Fedora 40のFlatpakアプリからmDNSで名前解決できない"
date: 2024-08-26 21:30 +09:00
tags:
    - tips
    - computers
    - linux
---

## TL;DR

Fedora 40のデフォルトインストール上で、FlatpakのアプリからmDNSを使うためには`systemd-resolved`と`NetworkManager`の設定が必要。

## 詳細

Fedora 40 Workstationのデフォルトインストールを前提とすると、FlatpakのアプリケーションはmDNSの名前解決ができない。たとえばFlatpak版のGoogle Chromeを使用している場合、NASなどへアクセスする際に`.local`ドメインを使えないということが起こる。この状況は、Terminalから`ping`は通るし、Files（GNOMEの場合）でアクセスすることもできる。`.rpm`でインストールさえしていればブラウザからもアクセス可能であることを用いて確認できる。

この場合、`systemd-resolved`と`NetworkManager`の両方で設定が必要となる。おなじみの`avahi-daemon`は使わない。

### systemd-resolved

```bash
$ sudo mkdir -p /etc/systemd/resolved.conf.d
$ sudo cp /usr/lib/systemd/resolved.conf /etc/systemd/resolved.conf.d/
$ sudo vim /etc/systemd/resolved.conf.d/resolved.conf
```

下記の二行をアンコメントした上で値を変更する。

```resolved.conf
MulticastDNS=yes
LLMNR=yes
```

### NetworkManager

```bash
$ sudo vim /etc/NetworkManger/conf.d/mdns.conf
```

下記の内容で新規ファイルを作成する。

```resolved.conf
[connection]
connection.mdns=1
```
