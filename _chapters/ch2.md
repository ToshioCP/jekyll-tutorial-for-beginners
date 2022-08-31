---
layout: chapter
title: Jekyllクイックスタート
description: Jekyllで簡単なサイトを構築してみる
chap: 2
---

## Jekyllのインストール

JekyllはRubyで書かれたアプリケーションですので、両方のインストールが必要です。

### Rubyのインストール

ディストリビューションのパッケージをインストールすれば十分ですが、最新版をインストールしたければRbenvを使ってください。
その方法は

- [RbenvのGitHubレポジトリ](https://github.com/rbenv/rbenv)
- [Rbenvの使い方](https://toshiocp.com/entry/2022/07/08/115814)

を参考にしてください

### Jekyllのインストール

ディストリビューションのパッケージでインストールするか、gemコマンドでインストールします。

```
$ gem install jekyll
```

## Jekyllクイックスタート

ここでは、sampleという名前のサイトを作ってみましょう。
ただし、一般に公開するのではなく自分のパソコン上で（これをローカルといいます）作ります。

次の例では、ユーザのホームディレクトリのトップから作業を開始しますが、別の場所でも同じことができます。

```
$ jekyll new sample
... ... ...
New jekyll site installed in /（ユーザ名）/sample. 
$ cd sample
$ nano Gemfile # エディタはnanoでなくても良い
（エディタで、Gemfileの最後に"gem webrick"を追加し、上書き保存する）
$ bundle install
... ... ...
Bundle complete! 8 Gemfile dependencies, 32 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
$ bundle exec jekyll serve
... ... ...
（ブラウザでlocalhost:4000を開くと作成されたサンプル・サイトが表示される）
（CTRL-Cでコマンドラインにもどる）
$
```

ここで作られたウェブページはminimaというテーマを使っています。
minimaはJekyllのデフォルトのテーマです。
ウェブページには、「トップページ」「Aboutページ」「ブログ」の3つがあり、「ブログ」は追加が可能です。

ウェブの内容をカスタマイズするには

- 「about.markdown」をカスタマイズする。このとき、冒頭の`---`で囲まれた5行は変更しない。
- ブログ記事は、「\_posts/2022-08-12-welcome-to-jekyll.markdown」と同じ形式で「\_post」ディレクトリに追加していく。
  - ファイル名は「YYYY-MM-DD-タイトル名.markdown」。YYYY-MM-DDは年月日。拡張子は「md」でもよい。
  ファイル名中のタイトル名は全角を避けるのが良い。
  また、スペースは使わず代わりにハイフンを用いる。
  - 最初に`---`で囲まれた部分をサンプルファイルと同様に書く。「layout: post」はそのまま、「title:」には記事タイトル名を記述。
  このタイトル名は全角文字でも良い。ファイル名のタイトルと一致させる必要はない。
「date:」には投稿日時を書き、最後に「+0900」（Tokyoのローカルタイムの時差）を入れる。「categories:」タグは無くても良い。
  - 2つめの`---`の下から記事をMarkdownで書く。
- 以後、同様のファイルを「\_post」に追加することで、記事を投稿できる。

カスタマイズ後に

```
$ bundle exec jekyll serve
```

として、「localhost:4000」をブラウザで確認すれば、変更が反映されているはずです。

これで構築が済むなら、開発者は天国にも登るような気持ちになりますよね。
なお、構築された内容は`_site`ディレクトリに保存されますので、それをウェブ・スペースにアップロードすれば公開できます。

これはブログの例でしたが、他の種類のウェブサイトも構築できます。
そのためには、Jekyllに対する深い理解が欠かせません。
