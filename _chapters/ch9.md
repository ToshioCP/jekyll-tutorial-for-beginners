---
layout: chapter
title: Liquidの概要
description: JekyllのHTMLテンプレート言語Liquidについて
chap: 9
---

この章では、Jekyllでサイトを作る上で中心となるLiquidを説明します。
Liquidには簡潔なドキュメントがありますので、参考にしてください。

<https://shopify.github.io/liquid/>

Jekyllは独自にLiquidの拡張をしています。
これについては次のページに説明があります。

<https://jekyllrb.com/docs/liquid/>

## Liquidの基本

LiquidはHTMLの中に埋め込みます。
その埋め込まれたHTMLをテンプレートと呼んでいます。
ちょうどphp、あるいはRailsのeRubyに似ています。
埋めこむ対象が3つあります（オブジェクト、タグ、フィルター）。

### オブジェクト

オブジェクトは、何らかの情報を持っているもので、変数によって参照されます。
例えば、`page`という変数はページの情報を持っているオブジェクトを指しています。
その情報はドット記法で参照します。

- `page.title` ページのタイトル
- `page.description` ページのディスクリプション

オブジェクト「page」につけたドットの後の部分をプロパティといいます。
titleやdescriptionはpageオブジェクトのプロパティです。
ページのフロントマターに記述されたものはプロパティになり、ドット記法で参照することができます。

オブジェクトをHTMLの中に埋め込んで表示するには二重波カッコを使います。

{%raw%}
```
{{ page.title }}
```
{%endraw%}

### タグ

タグは制御構造（分岐、ループ）、テンプレート（コメント、raw）、変数への代入などです。
他の言語のステートメントにあたるものです。
タグは原則として出力されません。
タグは波カッコとパーセントで囲みます。

{%raw%}
```
{% if page.title == "サンプル" %}
```
{%endraw%}

### フィルター

フィルターは縦棒`|`で左の出力を右に流すものです。
ちょうどシェル・スクリプトのパイプのようなものです。
出力結果はHTMLに表示することが多いので{%raw%}`{{  }}`{%endraw%}を使うことが多いです。

{%raw%}
```
{{ page.title | escape }}
```
{%endraw%}

パイプの右側にフィルターの動作を記述します。
escapeはHTMLで特殊な意味を持つ記号をエスケープします。
例えば&lt;を&amp;lt;と変更します。

フィルターには多くの種類があります。
また、他の言語における演算子やメソッドの役割をフィルターが担っています。

## Jekyllで使う変数

Jekyllで使える変数は[Jekyllのドキュメント](https://jekyllrb.com/docs/variables/)に書かれています。
変数はLiquidで使います。

グローバル変数は次のとおりです。

- site => サイトの情報。\_config.ymlに記述した情報が元になる
- page => ページの情報。ページのフロントマターに書かれた情報
- layout => レイアウトの情報。そのフロントマターの情報
- content => ページのコンテンツ
- paginator => パジネーター。ブログ記事がたくさんある場合など、それをページに分割するときに使う

siteが置かれるホストのURLを\_config.ymlに書いたとします。

```yaml
site: http://username.github.io/
```

この情報はドット記法を使って「site.url」で参照できます。

（注）`site.url`を設定してレイアウトやコンテンツ内で使う場合、画面遷移のリンクには使わないでください。
ローカルと公開サーバでurlが違うので、ローカルでの遷移が機能しなくなります。

グローバル変数の主なプロパティについては[Jekyllのドキュメント](https://jekyllrb.com/docs/variables/)を参照してください。

## Liquidのオペレーター

Liquidのオペレータはコントロールフロー（ifなど）やループ（for）の条件に使えます。

- ==
- !=
- &gt;
- &lt;
- &gt;=
- &lt;=
- or
- and
- contains => 左側の文字列が右側の文字列を含んでいればtrue

オペレータが複数ある場合は右から順に評価されます。
また、かっこ`(  )`で評価順を変更することは**できません**。

Liquidには算術演算子（加減乗除）がありません。
その代わりにフィルターを使います。

## タグ（Liquidの制御構造）

項目が多いので、ここでの説明は極めて簡単になっています。
詳しい説明は[Liquidのドキュメント](https://shopify.github.io/liquid/tags/control-flow/)を見てください。

### コントロール・フロー

コントロール・フローは条件分岐です。
他の言語と同様の条件分岐ができます。
コントロール・フローは次の2種類です。

- if/unless （条件）〜 elsif/else 〜 endif
- case（条件）when 〜 [when 〜] else 〜 endcase

### イテレーション（ループ）

ループをLiquidではイテレーションと呼んでいます。

- for （条件）〜 else 〜 endfor。
elseは条件が成り立たなかった時に出力される
- break => ループを中止して外にでる
- continue => ループの現在の回をスキップして次の回にすすむ
- forループにはlimit/offset/range/reversedのオプションをつけることができる
- forの中でcycle （項目のリスト）を使うことができる。
1回目はcycleの引数リストの最初の項目、2回めは2番めの項目・・・と出力される
- tablerow => ループの中で表の行を出力する（tdタグなどが自動的に付く）。
cols/limit/offset/rangeのオプションをつけることができる

tablerowはHTMLの表を作れるので便利です。
HTMLテンプレートであるLiquidらしい機能です。

### テンプレート

- comment 〜 endcomment => コメントとして扱われHTML出力されない
- raw 〜 endraw => Liquidの解釈をせずにそのまま出力される。
{%raw%}`{{  }}`や`{%  %}`{%endraw%}をそのまま出力したい場合に使う。
- liquid => 複数行にわたりLiquidの構文を書くことができる。
いちいち{%raw%}`{%  %}`{%endraw%}で囲まずにすむのがありがたい。
ただし、Liquidのバージョン5以降の機能で、GitHubではまだサポートされていない。
（GitHubの2022/8/30時点のLiquidバージョンは4.0.3）
- echo => タグの中で用いられ、echoの次の要素を出力する。
フィルターを使える
- render => 他のテンプレートファイルを読み込む

### 変数への代入など

- assign => 変数への代入をする
- capture => 変数への代入をする
- increment/ decrement => 引数に変数をとり、0/-1からはじめて変数を1ずつ増加／減少させ、その値をHTMLに出力する。

## JekyllのLiquidで良く用いられるフィルター

以下では、JekyllのLiquidで良く用いられるフィルターを説明します。
いくつかのテーマを調べ、使用頻度の高い順に並べたので、ある程度一般性のある順番になっていると思います。

### default

パイプの左側からの入力がnil、false、空文字列のいずれかであるときに引数の値を出力し、それ以外は入力をそのまま出力します。

{%raw%}
```
{{ page.title | default: 無題 }}
```
{%endraw%}

- ページタイトルが設定されていれば、その文字列を出力
- ページタイトルが設定されていなければ「無題」を出力

最も使われているフィルターです。

### relative_url

ベースURLを加えたURLにします。
Liquidでpage.urlを参照するときなどにrelative\_urlが必要になります。

詳しい説明が[第6章](ch6.html)の「home.html」にありますので、それを参照してください。

なお、ベースURLは\_config.ymlの中で定義します。

{%raw%}
```
_config.yml での定義
baseurl: /jekyll-tutorial-for-beginners

これにより、出力は次のようになる
{{ "/assets/image/abc.png" | relative_url }}
=> https://ホスト名/jekyll-tutorial-for-beginners/assets/images/abc.png
```
{%endraw%}

なお、baseurlのデフォルト値は空文字列です。

### absolute_url

文字列の先頭にsite.urlとsite.base_urlを加えます。
前提として\_config.ymlにurlを登録することが必要です。

```yaml
url: https://username.github.io
```

文字列の最後にスラッシュはつけません。
デフォルト値は`https://localhost:4000`です。

### escape

escapeはHTMLで特別な意味を持つ文字をエスケープします。

- &lt; ⇒ &amp;lt;
- &gt; ⇒ &amp;gt;
- &amp; ⇒ &amp;amp;

これ以外にもエスケープ文字はあるので、詳細はインターネットで調べてみてください。
とくに&lt;と>が現れるとHTMLタグと解釈されてしまうので、文字そのものを出力するにはescapeフィルターをかけてやります。

これは、テーマの開発者やサイトの構築者とコンテンツ入力者が別のときにとりわけ重要です。
例えばpage.titleにエスケープしなければならない文字が入っているかどうかはコンテンツ入力者しか分かりません。
開発者は特別な文字の有無に関わらずきちんとした出力を得るためにescapeを使います。

{%raw%}
```
{{ page.title | escape }}
```
{%endraw%}

### strip_html

HTMLタグを取り去りたいときに使います。
markdownifyを使う後に使うことが多いと思います。
marakdownifyの説明は後でします。

{%raw%}
```
{{ f.image_caption | markdownify | strip_html }}
```
{%endraw%}

「マークダウンをHTMLに変換してタグを取る」ということは、ベタなテキストを手に入れることになります。

### replace

文字列置換です。
1番めの引数を2番めの引数に置換します。

{%raw%}
```
{{ "暑いなあ" | replace: "暑い", "寒い" }}
=> 寒いなあ
```
{%endraw%}

### split

文字列を、引数をデリミタとして分割した配列にします。

{%raw%}
```
{% assign ary = "abc, def, ghi, jkl" | split: ", " %}
{% for s in ary %}
  {{ s }}
{% endfor %}
=>
abc
def
ghi
jkl
```
{%endraw%}

### markdownify

これは、Jekyllが拡張したフィルターです。
マークダウンで記述された文字列をHTMLに変換します。
マークダウンで書かれたデータを処理するケースで使います。

### date

タイムスタンプを別の形式の時刻表示に切り換えます。
形式の指定は引数の文字列で与えます。
形式には[strftime](https://www.ibm.com/docs/ja/zos/2.2.0?topic=functions-strftime-convert-formatted-time)が用いられます。

{%raw%}
```
{{ "2022-8-21" | date: "%Y 年 %m 月 %e 日" }}
=> 2022 年 08 月 21 日
```
{%endraw%}

### remove

引数の文字列を削除します。

{%raw%}
```
{{ "<p>abcd</p>" | remove: <p> | remove </p> }}
=> abcd
```
{%endraw%}
