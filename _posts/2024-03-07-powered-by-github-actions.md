---
title: "Github Pages / Github Actions / Jekyllでブログを作る"
date: 2024-03-07 20:00 +09:00
tags:
    - tips
    - computer
redirect_from:
    - /blog/powered-by-github-actions
---

このブログはJekyllで静的にページを生成し、Github Pagesへ公開するようにGithub Actionsを設定している。`.md`ファイルをアップロードするだけで、ブログが更新される。かつて私は`pelican`や`hugo`をローカルで実行してサイトを公開していたが、今のほうが断然楽だ。ローカルに`ruby`環境がなくても問題がない。

テーマには[sakura-jekyll](https://github.com/oxalorg/sakura-jekyll)を選択した。これは[sakura](https://github.com/oxalorg/sakura)というミニマルなCSSフレームワークに基づいたテーマだ。見た目がいいし、複雑な`class`設計をする必要もない。私はこのテーマに対していくつかの変更を加えたうえで使用している。

[squeuei/sakura-jekyll-modified](https://github.com/squeuei/sakura-jekyll-modified)

- cssをCDNにアップロードされているものへ変更
- クライアントの設定に合わせてダークモードで表示するように変更
- タグとカテゴリ機能の追加。
    - [jekyll-archives](https://github.com/jekyll/jekyll-archives)を使用
        - `Gemfile`を追加
        - `_config.yml`に項目を追加
        - Github Actionsのworkflowを[github公式のjekyllを実行するもの](https://github.com/squeuei/squeuei.github.io/blob/main/.github/workflows/jekyll.yml)へ変更
    - `archive.html`を作成
    - `/tag/index.html`、`/category/index.html`を作成
- 日付をISO8601形式で表示するように変更

シンプルな構成が特徴のテーマでなぜわざわざカスタムしてまでアーカイブ機能を追加したのか。読んだ作品の感想を書くなら、タグで一覧出来るほうが望ましいだろうと思ったからだ。実現できてしまったので、書くか……。
