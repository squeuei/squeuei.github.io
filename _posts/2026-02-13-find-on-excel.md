---
title: "Excelでfind"
date: 2026-02-13 22:00 +09:00
tags:
    - tips
    - computer
    - programming
---

Excelで任意の長さの連続したデータをスピルとして取得したいとき、ありますよね。`=A1:A100`のように書いてしまうと、たとえば20個しかデータがない時に余計なセルまでスピル範囲に含まれてしまうし、120個データがあった時に取りこぼす。データ長に合わせて自動的にスピル範囲を調整してほしい！　ではどうするか？

データが連続しておらず、値があるセルだけを取り出したいのであれば、`FILTER`と`ISBLANK`を使えば良い。これは多くの人が用いている素直なやり方だろう。

```Excel
=FILTER(A:A,NOT(ISBLANK(A:A)))
```

これはMatlabでいうところの論理インデックスとして使える。`NOT(ISBLANK(A:A))`の代わりに`A:A>0`で抽出することもできる。

Matlabでいうところの`find()`、つまり「TRUEとなる行番号を取得」したいならこれ。`SEQUENCE(N,,M,L)`はMatlabの`M:L:M+L*N`と等価。

```Excel
=FILTER(SEQUENCE(100),NOT(ISBLANK(A1:A100)))
```

`SEQUENCE`と入力範囲の関係を調整するのが面倒なら`LET`で。iの範囲を設定するだけで処理してくれる。

```Excel
=LET(
    i, A5:A100,
    l_min, MIN(ROW(i)),
    l, ROWS(i),
    FILTER(SEQUENCE(l,,l_min), NOT(ISBLANK(i)))
)
```

1行目からデータが連続して格納されているならばこれでいい。

```Excel
=INDEX(A:A,SEQUENCE(COUNTA(A:A)))
```

1行目がヘッダだったら？　`A:A`を範囲に置き換えればいい。確実にこれ以上データをいれることはない、という範囲を設定すればいいし、最悪は重くなるかもしれないけど範囲を`A2:A1048576`とかにすれば確実に取りこぼしがない。

```Excel
=INDEX(A2:A10000,SEQUENCE(COUNTA(A2:A10000)))
```

あるいは`SEQUENCE`の方で範囲を調整しても構わない。

```Excel
=INDEX(A:A,SEQUENCE(COUNTA(A:A)-1,,2))
```

上の例と同じように、`LET`を使えば入力範囲iとヘッダ行数n_headerを設定すればいいようにできる。

```
=LET(
    i, A:A,
    n_header, 2,
    INDEX(i,SEQUENCE(COUNTA(i)-n_header, , 1+n_header))
)
```

一般に適用される注意点として、`INDEX`の代わりに`INDIRECT`や`OFFSET`を使わない方がいいということがある。`INDIRECT`や`OFFSET`は[揮発性関数](https://learn.microsoft.com/ja-jp/office/client-developer/excel/excel-recalculation#volatile-and-non-volatile-functions "Excel の再計算 \| Microsoft Learn")に分類される。揮発性関数は引数が変化していなくても常に再計算の対象となる。ということはつまり、全然関係ないセルを編集したときにも余計な処理が走るということ。普通の使い方をする分には問題がないけれど、[Excelを濫用する場合]({% link  _posts/2025-01-08-building-a-cartesian-product.md %})には問題となりうるのだ。動作が重くて使い物にならなくなる。そんな処理をExcelにさせるなって？　本当にそのとおり。
