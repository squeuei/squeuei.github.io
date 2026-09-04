---
title: "もーっと！　Microsoft Excelの力を濫用する"
date: 2026-09-06 00:00 +09:00
tags:
    - tips
    - computer
    - programming
---

[悪夢]({% link _posts/2026-02-13-find-on-excel.md%})は終わらない。モダンExcel（ここではMicrosoft 365版Excelを指す）の暗黒面へようこそ。

## スピル取得

`index`を使って不揮発性関数でスピルを取得するやり方は、`take`を使えばもっと簡単にできる。そして、`xmatch`と組み合わせれば、列全体から値が含まれる一番大きな行数までのスピルを取得することができる。

```
=index(a:a, sequence(counta(a:a)))
```


```
=take(A:A, xmatch(false, isblank(A:A),0,-1))
```

もっと言えば、上の`take`ですらこれだけで済んでしまう。

```
=A:.A
```

## 正規表現

正規表現を使って、マッチする文字列があるかどうか、マッチする文字列の抽出、マッチした文字列の置換ができる。標準の検索ダイアログではできないのに！？

`regextest`、`regexextract`、`regexreplace`

使い方については、もはや説明不要だろう。これでもう一度VSCodeにデータを貼り付けて正規表現で置換し、元のシートに戻す、というステップを踏むことはなくなる。

## またふたたびのスピル行列

`let`, `lambda`, `map`, `filter`, `reduce`, `vstack`, `hstack`, `choosecols`, `chooserows`, `sequence`

ここら辺の*usual suspects*を組み合わせることで、1セルの中で複数行、複数列を出力に持つ処理を記述し切ることができる。たとえば、スピル配列と複数列の出力を持つ関数を使って以下のような処理が書ける。

```Excel
=drop(reduce("", sequence(30),lambda(acc, x, vstack(acc,hstack(average(chooserows(A1:Z30, x)), product(chooserows(A1:Z30, x))^(1/columns(chooserows(A1:Z30, x))))))),1)
```

要するに、各行について相加平均と相乗平均を求める表現だ。本来ならば`byrow`を使って書きたいところだが、`byrow`や`bycol`は複数の列/行を返す`lambda`を使えないので、仕方なしに最初に*無*をaccumulateする`reduce`で処理して、最後に*無*を`drop`してる。本当は`let`で`(A1:B30, x)`に名前をつける方が楽だし処理も早い。

Matlabのequivalent codeはこう。関数型言語ならもっとExcelに寄せて書くことができるだろう。というか、Excelが関数型言語の考え方に沿って記述できるようになった、という方が正しいのだけど。

```Matlab
result = [];
for i = 1:size(data,1)
    [a, b] = some_func(data(i,:));
    result = [result ; a, b];
end

function [a, b] = some_func(x)
    a = mean(x);
    b = prod(x)^(1/numel(x)) ;
end
```

これくらいはたいしたことない？　じゃあもっと遊んでみよう。簡単のためにヘッダ、空行、空列はないものとする。以下のようなデータがあるとする。

A | B
--- | ---
Alice | Good
Bob  | Good
Carol | Neutral
Chuck | Evil
Dan | Neutral 
Eve | Evil
Faythe 	| Good
Mallory | Evil
Walter | Good

A,B列以外のどこかのセルに下式を入力する。

```Excel
=let(
    data_matrix, A:.B,
    group_name, choosecols(data_matrix,2),
    member_name, choosecols(data_matrix,1),
    unique_groups, unique(group_name),
    mem_of_gr, drop(
        reduce("",unique_groups,
            lambda(acc, x,
                vstack(acc, transpose(filter(member_name, group_name=x)))
            )
        )
    ,1),
    hstack( unique_groups, ifna(mem_of_gr, ""))
)
```

すると以下の結果が得られるはずだ。

A|B |C|D |E
--- | ---| --- |---  |---
Good | Alice | Bob | Faythe | Walter
Neutral | Carol | Dan | |
Evil | Chuck | Eve | Mallory | 

[フハハ、怖かろう！](https://dic.pixiv.net/a/%E3%81%B5%E3%81%AF%E3%81%AF%E6%80%96%E3%81%8B%E3%82%8D%E3%81%86 "ふはは怖かろう (ふははこわかろう)とは【ピクシブ百科事典】")

VBA？　PowerQuery/PowerPivot？　私そういう難しい機能よくわからなくて……。
