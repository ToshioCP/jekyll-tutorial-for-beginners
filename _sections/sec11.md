---
layout: section
title: レイアウトの仕組み
description: Minimaのレイアウトを調べ、レイアウトの仕組みを理解する
chap: 3
sec: 11
---

この章では、Jekyllの特徴であるLiquidを使ったレイアウト設計について学びます。
Jekyllのレイアウトは、HTMLとLiquidテンプレート言語を組み合わせて作成されており、動的にコンテンツを生成することができます。

今回はMinimaがレイアウトで何をしているのかを調べることにより、Liquidの基本的な使い方とJekyllのレイアウトの仕組みを理解します。

## Minimaの置かれている場所

まず、インストールされたMinimaテーマのファイルがどこにあるかを確認しましょう。
Minimaの場所は、Bundlerで調べることができます。

```
$ bundle info minima
  * minima (2.5.1)
        Summary: A beautiful, minimal theme for Jekyll.
        Homepage: https://github.com/jekyll/minima
        Path: /home/（ユーザ名）/.local/share/mise/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/minima-2.5.1
        Reverse Dependencies: 
                github-pages (232) depends on minima (= 2.5.1)
$ cd  /home/（ユーザ名）/.local/share/mise/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/minima-2.5.1
$ ls
LICENSE.txt  README.md  _includes  _layouts  _sass  assets
$
```

表示されたPathのディレクトリ（Minimaのディレクトリ）に移動し、ディレクトリをリスティングしました。

さらに詳しく調べると、Minimaのディレクトリは以下のような構成になっています。

```
LICENSE.txt
README.md
_includes
  disqus_comments.html
  footer.html
  google-analytics.html
  head.html
  header.html
  icon-github.html
  icon-github.svg
  icon-twitter.html
  icon-twitter.svg
  social.html
_layouts
  default.html
  home.html
  page.html
  post.html
_sass
  minima
    _base.scss
    _layout.scss
    _syntax-highlighting.scss
  minima.scss
assets
  main.scss
  minima-social-icons.svg
```

この中にある`_layouts`ディレクトリの下にあるのが、レイアウトに関するファイルです。

## Jekyllのビルドとレイアウト

Jekyllがビルドをするとき、どのようにレイアウトするかを説明します。

1. ファイルのフロント・マターから、layoutキーをもつマッピング（例えば`layout: home`）を探す
2. ファイルのコンテンツに、Liquid、MarkdownのHTMLへの変換を、その順でおこなう
3. 得られたHTMLファイルを`content`というLiquidの変数に自動的に代入される
4. `/_layouts` ディレクトリの中から、指定されたレイアウトファイル（例：`home.html`）を探す。3の`content`変数の内容は保持される
5. レイアウトファイルにLiquidを適用する。そのとき、レイアウトファイルの {% raw %}`{{ content }}`{% endraw %}という記述は、ステップ2で作成したHTMLの中身で置き換わる
6. レイアウトファイル（例えば`home.html`） のフロント・マターに、別のレイアウト指定（例えば`layout: default`）があれば、`_layouts/default.html`をレイアウトとして、3から5までの手順を繰り返す
7. すべての入れ子構造が解消され、一つの完成したHTMLデータができあがると、それを `_site/` ディレクトリに書き出す

この説明にひとつ付け加えなければならない点があります。

**`/_layouts`に探しているレイアウトファイルが無いときは、テーマのディレクトリの`_layouts`を探しに行く。**

ということです。

`my-blog`のソース・ルートには`_layouts`ディレクトリがないので、Minimaのレイアウトが使われていました。

## Minimaのhomeレイアウト

それでは、Minimaのレイアウトファイル`home.html`を見てみましょう。

{% raw %}
```html
---
layout: default
---

<div class="home">
  {%- if page.title -%}
    <h1 class="page-heading">{{ page.title }}</h1>
  {%- endif -%}

  {{ content }}

（中略）

</div>
```
{% endraw %}

- フロント・マターでレイアウト`default`を指定しており、入れ子構造になっている
- {% raw %}`{%-` や `-%}` のようにハイフンを付けると、タグの前後にある改行や空白を削除する（トリミングする）{% endraw %}
- {% raw %}`{%- if page.title -%}`{% endraw %}はLiquidの`if`タグで、もし、`page.title`という変数が定義されていれば、{% raw %}`{%- endif -%}`{% endraw %}までの部分を出力する
- `page.title`は、ソースファイルのフロント・マターで`title: XXXX`という記述があれば`XXXX`に置き換えられる。`my-blog`の`index.md`にはこの記述がないので出力されていない
- {% raw %}`{{ content}}`{% endraw %}にはソース・ファイルの内容が代入される
- この後はブログ記事へのリンク・リストを生成する部分が続く（ここでは説明省略）

## Minimaのdefaultレイアウトとファイルのインクルード

レイアウト`default`は基本となるレイアウトで、他の3つのレイアウト`home.html`、`page.html`、`post.html`のすべてのレイアウトが指定しています。
`default`の機能は次の2つです。

- サイト全体に対するスタイルシートの適用
- ヘッダとフッタの生成

ファイルは短いので、その全体をここに示します。

{% raw %}
```html
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "en" }}">

  {%- include head.html -%}

  <body>

    {%- include header.html -%}

    <main class="page-content" aria-label="Content">
      <div class="wrapper">
        {{ content }}
      </div>
    </main>

    {%- include footer.html -%}

  </body>

</html>
```
{% endraw %}

この中で{% raw %}`{%- include head.html -%}`{% endraw %}というのは、`/_includes`ディレクトリの下にある`head.html`というファイルをここにインクルード（挿入）する、という意味です。
インクルードした3つのファイルの内容は次のようなものです。

- `head.html`: HTMLのヘッド・セクション（メタデータやスタイルシートなど）の記述
- `header.html`: サイトのヘッダの部分（サイトのタイトルなど）
- `footer.html`: サイトのフッタの部分（著者、SNSリンク、ディスクリプションなど）

## このセクションのまとめ

このセクションで見てきたように、Jekyllは、サイトのページを部分に分解、整理し、構造的にレイアウトを構成しています。
このような構造化はJekyllのみならず、一般にサイトを構築する際に有効な方法です。
Jekyllでは、それぞれのパーツを別ファイルに分けることもしているので、修正、保守がやりやすくなっています。

- フロント・マターの`layout`で指定されたファイルを`_layouts`ディレクトリ、またはテーマの`_layouts`ディレクトリ内のレイアウトファイルとして用いる
- `include`タグを用いて、部品をファイルにして整理する

このような構造を理解することによって、レイアウトのカスタマイズ、あるいはユーザ独自のテーマを作成することができます。