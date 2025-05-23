---
title: "JIS配列キーボードをUS配列として認識させて使う"
date: 2024-03-12 21:30 +09:00
tags:
    - keyboard
    - tips
    - computer
redirect_from:
    - /blog/use-a-jis-keyboard-as-a-us-layout
---

かつての私は、ラップトップはUS配列キーボードのものが一番良いと考えていた。一方、日本で販売されているノートPCの大多数はJISキーボードであり、自分好みの選択肢が少ないことを嘆いていた。しかしながら、最近になって考えを改めた。きっかけはMacbook Airを購入したことだ。

Apple製品は[公式の整備済製品](https://www.apple.com/jp/shop/refurbished)が販売されており、通常の新品価格と比較して若干安い価格で購入することができる。問題は、新品購入とは異なり、キーボード配列を実質的に選べないこと。Apple製品はJIS以外のキーボードを選んで買いやすいという魅力があるのだが、整備済製品に限って言うと、JISキーボード以外の製品がリストに現れることはごくまれである。私はキーボードに対するこだわりと価格とを天秤にかけ、後者を選んだ。

幸いなことに、MacbookのキーボードはJIS配列であっても一部キーのピッチやサイズが小さくなるということはない、スタンダードな配列だ。その意味で最低限はクリアできている。問題は、ここからどう快適にしていくかだ。macOSには[karabiner-elements](https://karabiner-elements.pqrs.org/)という定番ソフトがあり、その機能の一つであるVirtual Keyboardを使って、JISキーボードをUS配列として認識させている。そしてこれには大きな副次的効果があった。通常のJISキーボードと同じく“かなキー”・“英数キー”でIME切替できることが想像以上に快適だったのだ。これはUS配列では真似ができない。同じことが他のOSでもできないかと思って調べた結果が以下のリンクだ。要約するとWindowsでは`Powertoys`を、Linux (GNOME環境)では[Input method and touchpad shortcuts - GNOME Shell 拡張機能](https://extensions.gnome.org/extension/6066/shortcuts-to-activate-input-methods/)を使う。

[物理JISキーボードをUS配列として認識させ、かつ変換/無変換キーでIMEをON/OFFする方法](https://gist.github.com/squeuei/13d2987fc955ee0b7130ed9aec5265cb)

結果として、「テンキーレスであるにもかかわらずEnterの右に一列キーが存在する」、「キーのピッチやサイズが一部だけ小さくなる」というような明白な _地雷_ さえ回避すれば、JIS配列キーボード内蔵のラップトップを選んでも問題がない、むしろ快適にUS配列として利用できるということがわかった。これはいくつかの面で有利になる。一つは選択肢が増えること。基本的に日本国内で販売されるラップトップの大半がJIS配列であるため、選択肢が広がる。選択肢が広ければセールなど割安な価格での調達も可能になる。難点があるとすれば、故障した時の部品調達がUS配列より難しいということだろうか。最近はThinkpadでもキーボード交換が難しいモデルが増えているので、これさえもあまり価値のあることではないかもしれない。

余談だが、`karabiner-elements`では他に以下の置き換えを設定している。

- “CAPS LOCK” -> “Fn”
- “International 1”（“ろ”・“_”） -> “CAPS LOCK”
- “International 3”（“￥”） -> “`”・“~”

これに対応するLinuxでの設定方法は[この記事に]({% link _posts/2024-05-27-keyboard-remap-on-linux.md %})。
