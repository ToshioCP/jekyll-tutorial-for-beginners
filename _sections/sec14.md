---
layout: section
title: カテゴリー
description: カテゴリーの実装を通してLiquidを学ぶ
chap: 3
sec: 14
---

このセクションでは実際にLiquidを使ったソースを書いてみましょう。
プログラムを学ぶには、実際に書いてみるのが一番です。

ここでとりあげる実例は、`my-blog`のブログ記事をカテゴリー別に一覧表示するページです。

## ブログ記事のフロントマターの書き方

ブログ記事では、カテゴリーをフロントマターで設定できます。
設定には、`categories`または`category`キーを用いますが、それぞれは、複数または単数のカテゴリーの設定用です。
以下の5つの書き方と、それによって設定されるカテゴリーを比べてみましょう。
「設定されるカテゴリー」のカラムでは、分かりやすいようにダブルクォート（`"`）をつけました。

|フロントマターの記述|設定されるカテゴリー|
|:---|:---|
|`category: Church`|"Church"|
|`category: George Town`|"George Town"|
|`categories: George Town`|"George", "Town"|
|`categories: [George, Town]`|"George", "Town"|
|`categories: ["George Town", Church]`|"George Town", "Church"|

3つ目のスペースで項目を区切る書き方は、YAMLの書き方ではありません。
複数の書き方が覚えにくいという方は、`categories`キーとYAMLのシーケンス（リスト）書きだけを使うのもひとつの考え方です。

|フロントマターの記述|設定されるカテゴリー|
|:---|:---|
|`categories: [Church]`|"Church"|
|`categories: ["George Town", Church]`|"George Town", "Church"|

## カテゴリーに関するJekyllの仕組み

プログラムを書く前に、カテゴリーに関するJekyllの仕組みを知っておきましょう。
Jekyllのドキュメント（英語）の「変数」の部分には次のように書かれています。

`site.categories.CATEGORY`: CATEGORYというカテゴリーを持つすべてのブログ記事のリスト

非常に簡潔に書かれているので、例をあげて説明します。
あるサイトには、3つのブログ記事A、B、Cがあり、AとBのフロント・マターには`category: Church`と書かれていて、Cには無かったとします。
ドキュメントは、`site.categories`のChurchプロパティは`[A, B]`というリスト（配列）だといっているのです。

ところが、実際には`site.categories.Church`と書いても期待通りには動いてくれません。
正しくは、`site.categories["Church"]`と、ハッシュ（連想配列）の形でかかなければなりません。
このことは、Jekyll 3.10.0のドキュメントの「Writing posts」の「Displaying post categories or tags」に書かれたサンプル・プログラムで示されています。

また、Jekyll 3.10.0のドキュメントには明記されていないのですが、Jekyll 4.4.1のドキュメントの「Posts」の「Tags and Categories」では、`site.categories`を`for`ループに使えることが書かれています。
これは、Jekyll 3.10.0でも使えます。
すなわち、`site.categories`は、

```
[ ["カテゴリー名", [記事1, 記事2...]], ["別のカテゴリー名", [記事A, 記事B...]] ]
```

のネスト（入れ子構造）した配列になっているということです。

実用的には、後者の`for`ループの中で`site.categories`を使うのが効果的です。

## カテゴリーの一覧ページをつくる

`my-blog`にカテゴリー一覧ページを作ります。
ソースのルート直下に`categories.md`というファイルを作ります。
以下がそのテンプレートです。
非常にシンプルなコードで書かれています。

`c[0]`, `c[1]`などは、配列`c`の0番目、1番目の要素を表します。
「0番目」は日常では使いませんが、配列は、最初が「0番目」から始まります。

{% raw %}
```html
---
layout: page
title: カテゴリー
permalink: /categories/
---

{% assign sorted_categories = site.categories | sort %}
{% for c in sorted_categories %}
  <h3>{{ c[0] }}</h3>
  <ul>
    {% for p in c[1] %}
      <li>
        {{ p.date | date: "%Y/%m/%d" }} - <a href="{{ p.url | relative_url }}">{{ p.title | escape }}</a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
```
{% endraw %}

フロント・マターは、Aboutページに似ていますから、特に説明する必要はないでしょう。
コンテンツの方を説明します。

## forループ

{% raw %}
```
{% assign sorted_categories = site.categories | sort %}
{% for c in sorted_categories %}
    ... ...
    ... ...
{% endfor %}
```
{% endraw %}

一行目は、配列`site.categories`をアルファベット順にソートしています。
my-blogではカテゴリーに`Church`や`Mosque`などの英語を使っているので、このソートが生きてきます。
もし、日本語のカテゴリーを使うなら、このソートは意味がなくなるでしょう。
その場合は、読み仮名をカテゴリーに加え、五十音順にソートするなどの、より複雑な仕組みが必要になります。

forループは、配列`sorted_categories`から要素をひとつずつ取り出して、変数`c`に代入し`endfor`までの部分を繰り返します。
変数`c`には、カテゴリーと、「それを含むブログ記事の配列」が代入されます。

```
cの中身：　["カテゴリー名", [記事1, 記事2...]]
```

## ループ内のブロック

{% raw %}
```
  <h3>{{ c[0] }}</h3>
  <ul>
    {% for p in c[1] %}
      <li>
        {{ p.date | date: "%Y/%m/%d" }} - <a href="{{ p.url | relative_url }}">{{ p.title | escape }}</a>
      </li>
    {% endfor %}
  </ul>
```
{% endraw %}

まず、`c[0]`（カテゴリー名）をレベル３の見出し（`<h3>`）で表示します。

残りはforループです。

{% raw %}
```
{% for p in c[1] %}
```
{% endraw %}

`c[1]`はブログ記事の配列なので、`p`には各ブログ記事が順に代入されます。

forループの中では

{% raw %}
```
{{ p.date | date: "%Y/%m/%d" }} - <a href="{{ p.url | relative_url }}">{{ p.title | escape }}</a>
```
{% endraw %}

- ブログの投稿日を表示
- ハイフンを表示
- ブログへのリンクを「ブログのタイトル名」につけて表示

それら全体が`<ul>`と`<li>`のHTMLタグをつかった「順序なしリスト」として表示されます。

画面はこのようになります。

![カテゴリー一覧ページ]({{ "/assets/images/my-blog-categories.png" | relative_url }})

## このセクションのまとめ

Liquidを使うと、ウェブページの作成を自動化できます。
ブログ記事を追加しても、このカテゴリーのページは修正する必要がありません。
Jekyllがビルドするときに自動的に新しいブログ記事のカテゴリーを調べて追加してくれます。

Jekyllを使う利点をまとめてみましょう。

- テーマがあるので、一からサイトを構築しなくて済む
- 一度構築が終われば、記事などの一部を追加変更するだけで、他のページは自動的に更新される

すなわち、サイトのメンテナンスの負担が非常に軽くなります。
JekyllとLiquidの習得には、1か月程度あれば十分でしょう。
学習コストは多少かかりますが、一度習得すればウェブサイトの構築は非常に速くすすむことでしょう。
