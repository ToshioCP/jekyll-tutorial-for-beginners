---
layout: section
title: レイアウトのカスタマイズ
description: レイアウトをカスタマイズする方法
chap: 3
sec: 12
---

レイアウトのカスタマイズは、ウェブサイトの見た目を向上させるために重要です。
このセクションでは、レイアウトをカスタマイズする方法について、具体例をあげて説明します。

## my-blogのレイアウト修正

今回は、サイトのタイトルの前にロゴを入れたいと思います。
ロゴはペナン島の形をした画像を用意しました。
タイトル行に入れる場合、高さが40px程度が良い（ページによっては60px程度の場合もあり）のですが、画像は縦120px（60pxの2倍）でスタイルシートで40px表示にしました。
このように、ウェブに表示するサイズの2倍程度の画像を用意して、それをスタイルシートでサイズ調整すると鮮明な画像にすることができることが多いです。

画像ファイルの名前は`Logo-Penang.png`とし、ソースの`/assets/images`ディレクトリに入れておきます。

Minimaでは、サイトのタイトルはヘッダに表示されていますので、今回修正するのは`_includes/header.html`になります。

## 修正前の作業

ファイルを修正する前に、必ず次の作業をしてください。

1. `bundle info minima`を用いて、Minimaのディレクトリを確認する。
そのディレクトリの`_includes`ディレクトリに`header.html`がある
2. ソースディレクトリのルート直下に`_includes`ディレクトリを作成。
次に、ルート直下の`_includes/header.html`に、Minimaの`_includes/header.html`をコピーする

このコピーにより、`header.html`が2つでき、ひとつはMinimaの中、もうひとつはソースディレクトリの中にできます。
修正はソースディレクトリに新たにコピペしたファイルに対して行います。

なぜ、コピーされた方を修正するだけで上手くいくのでしょうか？
それには、Jekyllがレイアウトやインクルード・ファイルを探す仕組みが関係しています。
Jekyllは、まずルート直下のディレクトリ内、次にテーマのGemの下のディレクトリ内、という順でファイルを探し、ファイルが見つかった時点で探索を終了します。

- レイアウト： （１）`/_layouts`、（２）テーマGemの`_layouts`
- インクルード・ファイル： （１）`/_includes`、（２）テーマGemの`_includes`

したがって、ビルド時には、`/_includes/header.html`がインクルードされ、Minima内の`header.html`は使われないのです。

## ファイルの修正

`header.html`の最初の数行を示します。

{% raw %}
```html
<header class="site-header" role="banner">

  <div class="wrapper">
    {%- assign default_paths = site.pages | map: "path" -%}
    {%- assign page_paths = site.header_pages | default: default_paths -%}
    <a class="site-title" rel="author" href="{{ "/" | relative_url }}">{{ site.title | escape }}</a>
```
{% endraw %}

最後の行の{% raw %}{{site.title | escape }}{% endraw %}がサイトのタイトルです。
縦棒とescapeは、そのタイトルがHTMLのタグを含んでいたら、それをエスケープするというものです。
例えば`<`はそのままHTMLで表示できないので`&lt;`にエスケープします。
変数（例えば`site.pages`）をHTMLに出力するときには、縦棒＋escapeをつけておくのが良いです。

とにかく、これがタイトルなので、その前に画像のHTMLを入れます。
最後の行を次のように修正します。

{% raw %}
```html
 <a class="site-title" rel="author" href="{{ "/" | relative_url }}">
      <img src="{{ "/assets/images/Logo-Penang.png" | relative_url }}" alt="Penang" style="height: 40px; vertical-align: middle; margin-right: 10px;">
      {{ site.title | escape }}</a>
```
{% endraw %}

## ローカルでテスト

修正が終わったら、ローカルでテストをします。

```
$ bundle exec jekyll serve
```

ブラウザで`localhost:4000/my-blog`を見ると、タイトルの前にロゴが現れています。
ブログ記事やAboutページでも同様に見えていることを確認します。

## GitHubへのプッシュ

修正がOKであれば、コミットしてGitHubにプッシュします。

```
$ git add .
$ git commit -m "Add site logo to header"
$ git push
```

GitHub Pagesのサイトを開き、変更を確認します。
ページの生成には少し時間がかかることに注意してください。
また、ブラウザにキャッシュが残っていると変更が反映されないことがありますので、再読み込みしてください。

## このセクションのまとめ

レイアウトの変更は、直接テーマのレイアウト・ファイルを変更するのではなく、ソース・ディレクトリ内の対応するディレクトリにコピーし、コピーの方を変更します。
オリジナルを変更することのないよう、注意してください。

