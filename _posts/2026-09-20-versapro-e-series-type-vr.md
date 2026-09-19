---
title: "VersaPro E Series Type VR"
date: 2026-09-20 00:00 +09:00
tags:
    - tips
    - review
    - computer
    - linux
---

## The Most Important Thing

**2026-11-30で保証が切れるため、保証期間はこの記事を書いている時点から実質的に2ヶ月強しかない。**

私の買った個体について、保証書には「出荷年月　2025年10月」「保証期間　ご購入日から1年間又は（2026年11月迄）」と書かれている。おそらくは発売から5年で保守対象から外れるということだと思われる。購入から1年以内であっても、保証期間が終了するので注意。だからこそ投げ売りされている、とも言える。

## 本文

[VersaPro Eシリーズ タイプVR](https://support.nec-lavie.jp/e-manual/m/nx/vp/202106/html/ve_vr_1.html)は2021年に発売された、教育用途機をベースとする2-in-1ラップトップPCである。Intel Celeron N5100  (4C/4T)、RAM 8GB、eMMC 64GB、重量は11インチクラスにもかかわらず1.3kgほどあり、画面もIPS方式のマルチタッチディスプレイではあるものの1366×768という令和にあるまじき低解像度と、貧弱にも程があるスペック。

一応、いいとこ探しをすると、最近のChromebookでよくあるIntel N50とほぼトントンのCPU性能があり、RAMは多め、ディスプレイも視野角が広い。キーボードは不自然ではないJIS配列で、物理的なフットプリント*は*小さい。教育用途機がベースなので頑丈さには期待できる、あたりだろうか。

Windows機として運用するには性能が低く、特に更新すらできなくなるくらいにストレージが小さすぎる。正直に言って、買ってきたものはそのままで使いたいであろう、まともな人間が買うものではない。しかし、そんなマシンが[アマゾンで2万円を切って販売](https://weekly.ascii.jp/elem/000/004/434/4434847/ "前にも後ろにもカメラ!? 画面は10点タッチ対応、NECの11.6型「2 in 1ノート」が1万9800円 - 週刊アスキー")されていたのを見て、私はまともな人間ではないので買ってしまった。一応言い訳をしておくと、多少の勝算はあった。想定する用途はテキストエディタとあとはせいぜい動画鑑賞。なので高解像度はすごく重要というわけではない。ストレージの問題は、すぐにLinux化するつもりだったのであまり気にしていなかった。どちらかというとLinuxの互換性を心配していた（かつてLavie Direct PMで痛い目を見たので）。が、[YouTubeにアップロードされている数少ない動画の一つ](https://www.youtube.com/watch?v=AgIsPD9rO4Q "プライムデーで購入した激安21,000円ダメダメノートＰＣ!! Ubuntu導入で超快適!! - YouTube")に、Ubuntuでバッテリ残量が表示されているのを見つけた。あと、ほぼ同じ筐体でChromebookがラインナップされていたことも、弱い後押しとなった。これらの状況証拠から、Linuxで動かすことはきっと大丈夫だろうと判断し、購入に踏み切ったわけだ。

UEFIを開けばFn-Ctrl SwapやF1-F12を主機能に設定することができるのでご安心を。（無駄だとは思うけど）NEC恒例の一度だけしか作成できないリカバリメディア作成を行った後、Debianをインストール。Fedoraにしなかったのはこのマシンが[Jasper Lake世代](https://en.wikipedia.org/wiki/Tremont_(microarchitecture)#Mobile_processors_(Jasper_Lake) "Tremont (microarchitecture) - Wikipedia")のCPUだから。この世代は[`x86-64-v2`](https://ja.wikipedia.org/wiki/X64#%E3%83%9E%E3%82%A4%E3%82%AF%E3%83%AD%E3%82%A2%E3%83%BC%E3%82%AD%E3%83%86%E3%82%AF%E3%83%81%E3%83%A3%E3%81%AE%E4%B8%96%E4%BB%A3 "x64 - Wikipedia")に分類されるため、[CERNがDebianに移行した理由](https://atmarkit.itmedia.co.jp/ait/articles/2609/12/news005.html "CERNが巨大加速器の制御基盤OSを「CentOS 7」から「Debian 13」に全面移行　コミュニティー主導OSを選ぶ理由：「CentOS Stream」追従を断念 - ＠IT")がそのままあてはまるのだ。FedoraはRedHatでもCentOSでもないにしても、方針は近づいていくかもしれない。デスクトップ環境はGNOMEを選択。使い慣れているKDEや、リソースが節約できるXfce4、LXQtにしなかったのは、一応このPCが2-in-1であり、タブレットモードにスムーズに移行できそうなのはGNOMEだけだったからだ。バトルというほどのこともなく、他のPCと同じくらいスムーズにインストールできる。

起動時、`dmesg`にはなんらかのエラーが出ているけど、バッテリは残量含め認識されてるし、Fnキーを組み合わせるキーボードショートカットも動作する。縦横認識も、タブレットモードにしたときのキーボード無効化も、タッチスクリーンも動く。動画再生も音声出力も大丈夫だ。とりあえずの問題は生じていないように見える。キーボードは安っぽいけど、キーピッチはそれなりに確保されているのでミスタイプは少ない。VSCodeなどのアプリをインストールしても消費は20GB程度で、まだ30GB程度の余裕はある。Firefoxで1タブだけ開いて、動画再生しながらVSCodeで日本語入力しているとき、RAMは7.5GB中5GB程度の消費。4GBだと破綻していたのでギリセーフ。バッテリの保ちは、そこまで良くはないかも。バックグラウンドで動画再生とか欲張らなければ5時間くらいだろうか。

[ASUS C100PA Chromebook](https://www.asus.com/jp/laptops/for-home/chromebook/asus-chromebook-flip-c100/ "ASUS Chromebook Flip C100PA \| ノートパソコン \| ASUS 日本")を[crouton](https://github.com/dnschneid/crouton "dnschneid/crouton: Chromium OS Universal Chroot Environment (EOL)")や[Arch Linux ARM](https://archlinuxarm.org/)で運用していたころを思い出す。あれはあれで楽しかった。C100PAと比べると、VersaProは重いし安っぽい、省電力性能でも負けるけど、それでも性能的な優位性はあるし、これだけのスペックがあれば用途を限定すれば使えないこともない。馬鹿と鋏は使いよう、破れ鍋に綴じ蓋、[置かれた場所で咲きなさい](https://www.gentosha.co.jp/book/detail/9784344021747/ "『置かれた場所で咲きなさい』渡辺和子 \| 幻冬舎")ということか。
