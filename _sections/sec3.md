---
layout: section
title: Jekyllによるビルドとは
description: Jekyllがビルドで何をしているのかを理解する
chap: 1
sec: 3
---

## ディレクトリ`_site`

前回、`bundle exec jekyll serve`で、ソースをビルドし、ローカル・サーバを起動するところまでやりました。
ところで、ソースのビルドの前後で、`my-blog`ディレクトリにある変化があったことに気づいたでしょうか。
それは、新たに`_site`というディレクトリができたということです。

Jekyllはソースディレクトリのファイルから、ウェブページを組み立て（これをビルドといいます）、作られたリソース（HTMLやCSS、画像など）を`_site`に保存していたのです。
ビルド前のファイルを「ソースファイル」、ビルド後のファイルを「生成ファイル」といいます。
ユーザが編集するのはソースファイルの方です。
逆に、ユーザが`_site`内を編集することはありません。
ビルドの直前に、`_site`内のすべてのファイルは削除されるからです。

ソースファイルと生成ファイルのパスを比べてみましょう。

|ソースファイル|生成ファイル|
|:---------------|:--------------|
|`404.html`|`_site/404.html`|
|`Gemfile`|生成されない|
|`Gemfile.lock`|生成されない|
|`_config.yml`|生成されない|
|`_posts/2026-01-26-welcome-to-jekyll.markdown`|`_site/jekyll/update/2026/01/26/welcome-to-jekyll.html`|
|`about.md`|`_site/about/index.html`|
|`index.md`|`_site/index.html`|

この表でソースファイルと生成ファイルを比較してみると、次のことがわかります。

- MarkdownファイルはHTMLファイルに変換されている
- ソースファイルと生成ファイルのパス構成が変わっている
- 生成されなかったソースファイルがある

## ファイルの変換

ビルドによるファイルの変換について、さらに見ていきましょう。

変換は、ファイルの種類によって異なります。

|ファイルの種類|変換|
|:----------|:-----|
|Markdown|Liquidの展開=>MarkdownからHTMLへ変換|
|HTML|Liquidの展開|
|SCSS|CSSへ変換|
|その他（画像など）|変換せずそのまま出力|

この表からJekyllのソースファイルを書くには、次のことの理解が望ましいことがわかります。

- Liquid
- Markdown
- SCSS

Liquidについては、[セクション15]({% include get_sec_url.html sec=15 %})で学習します。
MarkdownとSCSSについては、このチュートリアルではとくに説明しません。
必要に応じ、インターネット上の解説サイトを参照してください。

- [はてなブログのMarkdown徹底解説][1]
- [【Sass入門】SCSSの基本的な書き方8選！コード付きで解説][2]

## テーマ

前項で述べたように、Jekyllのソースファイルを書くには、「Liquid」「Markdown」「SCSS」の理解が必要ですが、それらの理解が十分でなくてもある程度ソースコードを書くことができます。
それは、Jekyllにはテーマというものがあり、それがLiquidやSCSSの複雑な部分を肩代わりしてくれ、開発の大きな手助けになるからです。

今回作成している`my-blog`ではMinimaというテーマを使います。
MinimaはJekyllのデフォルトのテーマで、基本的な機能に限ったシンプルなものです。
Jekyllの基本が理解できたら、テーマの機能についても学習してみてください。
その際は、まずはシンプルなMinimaのソースを読むことから始めると良いと思います。

ところで、GitHub Pagesで使えるテーマは他にもあり、[GitHub Pagesのドキュメントページ][3]で公開されています。
異なるテーマを使うと、レイアウトが変わるだけでなく、サイトの構成も変わります。
サイト構築の実践段階では、いろいろなテーマを試してみると良いでしょう。

[1]: https://toshiocp.hatenablog.com/entry/2022/07/09/235226
[2]: https://www.kagoya.jp/howto/it-glossary/web/scsswriting/
[3]: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll#supported-themes