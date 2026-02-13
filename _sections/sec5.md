---
layout: section
title: Aboutページの作成
description: Aboutページをカスタマイズする
chap: 1
sec: 5
---

## about.mdの確認

まずは、`jekyll new`で生成されたテンプレートの確認です。
`about.md`をエディタで開きます。

```
---
layout: page
title: About
permalink: /about/
---

This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](https://jekyllrb.com/)

You can find the source code for Minima at GitHub:
[jekyll][jekyll-organization] /
[minima](https://github.com/jekyll/minima)

You can find the source code for Jekyll at GitHub:
[jekyll][jekyll-organization] /
[jekyll](https://github.com/jekyll/jekyll)


[jekyll-organization]: https://github.com/jekyll
```

## フロントマター

このフロントマターは`index.md`とは異なります。

- レイアウトが`page`になっている（`index.md`のレイアウトは`home`）
- タイトルがある
- `permalink`がある

Aboutページは、そのサイトを説明するためのページで、その説明内容は変わることがありません。
そのようなものを「ページ」または「固定ページ」といい、`page`は、それに適したレイアウトを提供します。
`page`レイアウトにも`home`のようなヘッダーとフッターがありますが、ブログへのリンク・リストはありません。

フロントマターのタイトルは、このページ自身のタイトルです。
`_config.yml`に書かれたタイトルは、サイト全体のタイトルで、ページのタイトルとは異なることに注意しましょう。
このタイトルは「ペナン島の紹介」に変更しましょう。

Permalinkはリンクに関するコマンドです。
Permalinkの説明の前に、ディレクトリとアドレスについて説明します。

### ディレクトリとアドレスの関係

permalinkの設定が無い場合をまず説明します。

ソースファイルはルートディレクトリの下に書かれます。
例えば`about.md`はルートディレクトリの直下にあります。
それをパス名で`/about.md`と表すことにします。

Jekyllでビルドすると、HTMLファイルが`_site`ディレクトリの下に作成されます。
フロントマターのPermalinkの指定がなければ、`_site`の直下にファイル`about.html`が生成されます。
それをパス名で`/about.html`と書きます。
この最初のスラッシュは`_site`ディレクトリを指しています。

Jekyllでローカルのウェブサービスを起動し、ブラウザで見るときは、アドレスが次のようになります。

```
http://localhost:4000/about.html
```

このアドレスを分解すると、

- http: スキーム名（通信方法など）
- localhost: ホスト名（サーバー名）
- 4000: ポート番号（通常のウェブサービスはポート番号が80で省略可だが、Jekyllのローカルサービスでは4000を使い省略はできない）
- /about.html: パス名

ウェブサイトを一般に公開する場合はホスト名がインターネット上のアドレスになります。

URL上のパス名と、`_site`以下のパス名がぴったりと一致するというのが、ディレクトリとアドレスの関係ということになります。

### Permalink

次にpermalinkを設定した場合です。

Permalinkは、出力先のURLを指定します。

```
permalink: /about/
```

このように書くと、出力先のアドレスが変わります。

|permalinkなし|permalinkあり|
|:-----|:-----|
|`/about.md`|`/about/`|

アドレス`/about/`の最後にスラッシュがあるということは、`about`はディレクトリ名であるということです。
このとき、ウェブサーバーはそのディレクトリ下の`index.html`を表示する、というルールがあります。
したがって、ファイルのパス名としては、`/about/index.html`（`_site`をルートと見ています）になります。

Permalinkのような、一見複雑なことをなぜするのか、というのはもっともな疑問ですが、これには理由があります。
`/about/`と書くと、サーバーは`/about/index.html`（HTML）だけでなく、`about/index.php`（PHP）などにも対応できるという利点があります。
ただし、JekyllではPHPは使いませんが。
一般的なURLについての考え方に`/about/`が優れているというものがあるということです。

参考

- [Cool URIs don't change](https://www.w3.org/Provider/Style/URI)
- [クールなURIは変わらない](https://www.kanzaki.com/docs/Style/URI.html)

## コンテンツ

コンテンツ（フロントマター以外の部分）をブログの内容に相応しいものに変更します。

ソースファイル`about.md`はMarkdownのファイルです（拡張子`.md`はMarkdownのもの）。
Markdownにはオリジナルのものと、それを拡張したものなど、いくつかの方言があります。
Jekyllのデフォルトは[kramdown](https://kramdown.gettalong.org/)というマークダウンです。

kramdownは、ほぼGitHubのMarkdown（GFM）を変換できます。
細かい点での違いは[kramdownの文法](https://kramdown.gettalong.org/syntax.html)を参照してください。

以下のように`about.md`を変更します。
またペナンのレストランの「点心」の写真を`/assets/images/DimSum.jpg`として保存し、`about.md`の最後で取り込んでいます。

```
---
layout: page
title: ペナン島の紹介
permalink: /about/
---

ペナン島はマレー半島の北西部に位置し、東西貿易の拠点として、歴史にその名を刻みました。
特にユネスコ世界遺産に登録されたジョージタウンは、マレー、中国、インド、イギリス（かつての宗主国）の多様な文化に彩られた街並みで有名です。

また、ペナンは「食の都」としても有名です。
名物の「アッサムラクサ」や「チャークイティオ」などは、ホーカー（屋台）で、安く、美味く食べることができます。

近年では、街の至るところに描かれたストリートアートも、旅行の楽しみとなっています。
ホームページの画像はその代表的なアートです。

## 観光も良し、住むにも良し

ペナン島は、単なる観光地にとどまらず、「住みたい場所」としても高い人気を誇ります。
英語が広く通じるためコミュニケーションのストレスが少なく、医療水準も高いため、日本人のロングステイ先としても常に上位にランクインしています。
生活コストが低いことも理由のひとつでしょう。
都会的な便利さと、南国特有ののんびりとした時間の流れが同居している点が、多くの人を惹きつけて止まないのです。

## ペナン島へ行ってみよう

ペナン島は、訪れる人の目的に合わせて様々な表情を見せてくれます。
ノスタルジックでエネルギッシュなこの島へ、ぜひ一度足を運んでみてはいかがでしょうか。
きっと、忘れられない思い出を作ることができるでしょう。

![点心]({{ "/assets/images/DimSum.jpg" | relative_url }})
```

コンテンツ内の見出し（ヘディング）を`##`（HTMLの`<h2>`）で始めているのは、生成ファイルではフロント・マターのタイトルが`<h1>`を使っているからです。
