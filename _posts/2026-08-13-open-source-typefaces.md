---
title: "MonotypeのEULAに縛られずに高品質の書体を使う方法"
date: 2026-08-13 02:00 +09:00
tags:
    - book
    - computer
    - documentation
    - font
    - tips
---

出版、印刷、PDF用途を前提として、有名どころの買い切り欧文書体ってだいたい[Monotype系のEULAが](https://squeuei.github.io/old-blog/post/2017-11-16/%E8%A6%8F%E7%B4%84%E3%81%A8%E6%B4%BB%E5%AD%97/)[適用されるので](https://squeuei.github.io/old-blog/post/2019-02-24/%E5%80%8B%E4%BA%BA%E7%9A%84%E6%AC%A7%E6%96%87%E3%83%95%E3%82%A9%E3%83%B3%E3%83%88%E9%81%B8%E5%A5%BD/)、その軛を回避しつつ、たとえば商用PDF文書をそこそこな見た目にしたい人のためのメモ。

これはざっくりエディションなので、Web Font、`.mobi`、`.epub`、`.exe`などで使いたい人、受け渡しとかで神経使いたくない人は、Open Source――特に[SIL OFL](https://openfontlicense.org/)かせいぜい[LPPL](https://www.latex-project.org/lppl/)――にこだわるか、ご自身でもっと詳しく調べるか、あるいは強く生きてください。

## Proprietaryでも許容する

### Adobe Fontsを使う

[リリース時に契約していれば基本埋め込もうがデザインに使おうが大丈夫。](https://helpx.adobe.com/jp/fonts/using/font-licensing.html)

### Win / mac / MSOffice のバンドル書体を使う

[macOSバンドルのフォントはまあだいたいライセンス的に安全らしい。](https://discussions.apple.com/thread/255702321?sortBy=rank)[MSもよう気にしとる。](https://learn.microsoft.com/en-us/typography/fonts/font-faq "Font redistribution FAQ - Typography \| Microsoft Learn")


[この](https://learn.microsoft.com/ja-jp/windows/deployment/windows-missing-fonts?pivots=windows-11 "Windows をアップグレードした後に不足しているフォントを追加する方法 \| Microsoft Learn")[あたり](https://learn.microsoft.com/en-us/typography/font-list/ "Font library - Typography \| Microsoft Learn")をみるとなかなか魅力的

> Pan-European 補助フォント: Arial Nova, Georgia Pro, Gill Sans Nova, Neue Haas Grotesk, Rockwell Nova, Verdana Pro

### FontspringでWorry-free licensingの製品を買う

[寛容なライセンスで売ってくれる。](https://www.fontspring.com/worry-free)ここでしか買えない[Adobeの書体](https://www.fontspring.com/foundry/adobe)はWorry-freeじゃないけど。

### 日本語書体は買い切りで買う

日本語書体ならDTP/デザイン用途であればまだライセンスがユーザに有利（元フォントワークスを除く）。

## Going Open Source

書体自体の品質と、代替できる書体のカバレッジとを勘案してこんな感じ？　金のある企業や組織によって作られた書体は品質がある、という偏見。独断とえこひいきの連続。

### Commissioned by a company

- [Source Sans](https://github.com/adobe-fonts/source-sans) / [Source Serif](https://github.com/adobe-fonts/source-serif) (Adobe)
- [IBM Plex Sans / Serif](https://github.com/IBM/plex) (IBM)
- [Overpass](https://github.com/RedHatOfficial/Overpass) (RedHat)
    - Highway Gothic
- [Red Hat Text / Display / Mono](https://github.com/RedHatOfficial/RedHatFont) (RedHat)
- [Crimson Pro](https://github.com/Fonthausen/CrimsonPro) (Google)
    - Minion-*ish*
- [Literata](https://github.com/googlefonts/literata) (Google)
- [Newsreader](https://github.com/productiontype/Newsreader) (Google)
- [Spectral](https://github.com/productiontype/spectral) (Google)
- [FiraGo](https://github.com/bBoxType/FiraGO) (Mozilla Corp. / Telefonica / HERE Technologies)
    - FF Meta-*ish*
- [D-DIN](https://github.com/amcchord/datto-d-din) (Datto)
    - DIN1451-*ish*

### Commissioned by a country / organization

- [Charis](https://github.com/silnrsi/font-charis) (SIL)
- [Gentium](https://github.com/silnrsi/font-gentium) (SIL)
- [STIX Two](https://github.com/stipub/stixfonts) (STIX)
- [Zilla Slab](https://github.com/mozilla/zilla-slab) (Mozilla Foundation)
- [Public Sans](https://github.com/uswds/public-sans) (USWDS, US)
- [Baskervville](https://github.com/anrt-type/anrt-baskervville) (ANRT, FR)
- [Luciole](https://luciole-vision.com/#download) (CTRDV, FR)
    - Frutiger-*ish*
    - Attributeって書籍やポスターのどこに書くんだ？
    - CC BYが制約になるなら雰囲気は違うけど[Atkinson Hyperlegible Next](https://github.com/googlefonts/atkinson-hyperlegible-next) (Braille Institute, US)
- [PT Sans](https://fonts.google.com/specimen/PT+Sans/) / [PT Serif](https://fonts.google.com/specimen/PT+Serif) / [PT Mono](https://fonts.google.com/specimen/PT+Mono) (RU)

### Supported by the TeX / OSS Community

- [Liberation Fonts](https://github.com/liberationfonts/liberation-fonts)
- [Libertinus](https://github.com/alerque/libertinus)
    - Sans : Optima-*ish*
- [GUST TeX Gyre](https://www.gust.org.pl/projects/e-foundry/tex-gyre/whole)
    - Pagella : Palatino
    - Heros : Helvetica
    - Termes : Times
    - Adventor : Avant Garde Gothic
    - Bonum : Bookman
    - Chorus : Chancery
    - Schola : Century Schoolbook
    - Cursor : Courier
    - LPPL系だけどまあ大丈夫でしょう
- [EB Garamond](https://fonts.google.com/specimen/EB+Garamond)
- [DejaVu Fonts](https://github.com/dejavu-fonts/dejavu-fonts)

### Others

- [Inter](https://github.com/rsms/inter/)
- [Alegreya](https://github.com/huertatipografica/Alegreya)
- [Vollkorn](https://github.com/FAlthausen/Vollkorn-Typeface)
- [Junicode](https://github.com/psb1558/Junicode-font)
- [Cabin](https://github.com/impallari/Cabin)
    - Johnston / Gill Sans -*ish*

### Japanese

組版の観点から日本語は[Source Han Sans](https://github.com/adobe-fonts/source-han-sans/tree/master) / [Source Han Serif](https://github.com/adobe-fonts/source-han-serif/tree/master)とその派生一択か。[BIZ UDゴシック / BIZ UDPゴシック](https://github.com/googlefonts/morisawa-biz-ud-gothic) / [BIZ UD明朝 / BIZ UDP明朝](https://github.com/googlefonts/morisawa-biz-ud-mincho)も品質は高いが場所を選ぶ。
