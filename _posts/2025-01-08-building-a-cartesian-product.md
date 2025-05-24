---
title: "スピル配列を使って一つの式でデカルト積を作る on Microsoft Excel"
date: 2025-01-08 21:30 +09:00
tags:
    - tips
    - computer
    - programming
---

> [How to build a cartesian product from vectors in a single formula with spilled array.](https://gist.github.com/squeuei/6fa8ef4ce2783ea7ada4c0a21f0fd394)
>
> ## スピル配列を使って一つの式でデカルト積を作る
>
> 複数の行ベクタからなる集合のデカルト積（直積集合）を構築したいとき、スピル配列のおかげで以下の式を使えば出来る。新しめのExcelが必要かも。入力ベクタの列数は揃っていなくても動作する。もちろん入力は`VSTACK`を使って構築してもいいし、可能なら[スピル範囲演算子](https://support.microsoft.com/ja-jp/office/%E3%82%B9%E3%83%94%E3%83%AB%E7%AF%84%E5%9B%B2%E6%BC%94%E7%AE%97%E5%AD%90-3dd5899f-bca2-4b9d-a172-3eae9ac22efd)を使ってもよい。入力データにカンマが含まれている場合は、 `&","&`と`TEXTSPLIT(B,",")` の両方のカンマをなんか適当な文字に置き換えて使うことができる。
>
> ```Excel
> =LET(
> INPUT, A1:M2,
> CARTESIAN_PRODUCT, DROP(REDUCE("",REDUCE(TRANSPOSE(CHOOSEROWS(INPUT,1)), SEQUENCE(ROWS(INPUT)-1,,2),LAMBDA(X,Y, TOCOL(X &","&CHOOSEROWS(INPUT,Y)))),LAMBDA(A,B, VSTACK(A,TEXTSPLIT(B,",")))),1),
> FILTER(CARTESIAN_PRODUCT,BYROW(CARTESIAN_PRODUCT="",LAMBDA(X,NOT(OR(X)))))
> )
> ```
>
> ### 例
>
> [直積集合 - Wikipedia](https://ja.wikipedia.org/w/index.php?title=%E7%9B%B4%E7%A9%8D%E9%9B%86%E5%90%88&oldid=99957112)の例を用いる。
>
> Input:
>
> |♠|♣|♥|♦| | | | | | | | | |
> |:----|:----|:----|:----|:----|:----|:----|:----|:----|:----|:----|:----|:----|
> |1|2|3|4|5|6|7|8|9|10|11|12|13|
>
> (♠がA1セルにあるとする)
>
> Output:
>
> |Suits|Ranks|
> |:----|:----|
> |♠|1|
> |♠|2|
> |♠|3|
> |♠|4|
> |♠|5|
> |♠|6|
> |♠|7|
> |♠|8|
> |♠|9|
> |♠|10|
> |♠|11|
> |♠|12|
> |♠|13|
> |♣|1|
> |♣|2|
> |♣|3|
> |♣|4|
> |♣|5|
> |♣|6|
> |♣|7|
> |♣|8|
> |♣|9|
> |♣|10|
> |♣|11|
> |♣|12|
> |♣|13|
> |♥|1|
> |♥|2|
> |♥|3|
> |♥|4|
> |♥|5|
> |♥|6|
> |♥|7|
> |♥|8|
> |♥|9|
> |♥|10|
> |♥|11|
> |♥|12|
> |♥|13|
> |♦|1|
> |♦|2|
> |♦|3|
> |♦|4|
> |♦|5|
> |♦|6|
> |♦|7|
> |♦|8|
> |♦|9|
> |♦|10|
> |♦|11|
> |♦|12|
> |♦|13|
>
> 参考にしたサイト [（Excel）TEXTSPLIT関数をスピル（複数のテキストに適用）させる - いきなり答える備忘録](https://www.officeisyours.com/entry/2022/12/09/070752)

Excelってすげぇや。
