---
layout: section
title: Jekyllクイックスタート
description: Jekyllで簡単なサイトを構築してみる
chap: 1
sec: 2
---

## Jekyllのインストール

JekyllはRubyで書かれたアプリケーションですので、RubyをインストールしてからJekyllをインストールします。

### miseとRubyのインストール

執筆時点で、GitHub PagesでサポートされているRubyのバージョンは、3.3.4です（Rubyの最新版は4.0.1）。
Linuxディストリビューションが提供するRubyのバージョンは3.3.4とは限りませんので、複数バージョンのRubyを管理できるmiseまたはrbenvを使いましょう。

このチュートリアルではmiseを使ってのインストールを説明します。

miseはディストリビューションのパッケージに入っていない可能性がありますので、以下の方法でインストールしてください。
端末（ターミナル）から以下を実行します。

```
$ curl https://mise.run | sh
```

これで、miseがユーザー領域に（具体的には`~/.local/bin`の下に）インストールされます。
miseを有効にするために、一度ターミナルを再起動する必要があります。

次にRubyをインストールします。
端末から以下を実行します。

```
$ mise install ruby@3.3.4
```

miseの詳細は、[miseのウェブサイト][1]を参照してください。

## my-blogディレクトリの作成とRubyバージョンの指定

このチュートリアルでは、`my-blog`というディレクトリを作成し、そのディレクトリにローカル・サイトを構築します。
まず、ディレクトリを作成し、そこにカレントディレクトリを移動します。

```
$ mkdir my-blog
$ cd my-blog
```

このディレクトリで使用するRubyのバージョンを3.3.4のRubyに設定します。

```
$ mise use ruby@3.3.4
```

このコマンドを実行すると、`mise.toml`というテキスト・ファイルができます。
`cat`コマンドを使い、ファイルの中身を表示しましょう。

```
$ ls
mise.toml
$ cat mise.toml
[tools]
ruby = "3.3.4"
$ ruby -v
ruby 3.3.4 (2024-07-09 revision be1089c8ec) [x86_64-linux]
```

最後の行でRubyのバージョンが3.3.4であることを確認できました。

## .ruby-version

`mise.toml`はmiseでRubyをインストールしたときに有効ですが、他の方法でインストールした場合には機能しません。
mise以外にもrbenvというインストーラが有名ですが、その場合は`.ruby-version`というファイルを使います。
`my-blog`を公開して他のユーザと共有する場合には`.ruby-version`もディレクトリに含めておくと親切です。
以下のようにして`.ruby-version`を作成しておきましょう。

```
$ echo 3.3.4 >.ruby-version
$ ls -a
.ruby-version mise.toml
$
```

## Gemfileの作成とGemのインストール

このディレクトリで使うRubyのライブラリ（Gem）を記述するGemfileという名前のテキストファイルを作ります。
VS Codeなどのエディタで以下の内容を作成し、`Gemfile`の名前で保存してください。

```
source "https://rubygems.org"

ruby "3.3.4"

gem "github-pages", "~> 232", group: :jekyll_plugins
```

- `"https://rubygems.org"`はRubyGemsというRubyライブラリのウェブを指すアドレス。ここをGemインストールの共有元として指定
- Rubyのバージョンを3.3.4に指定
- `github-pages`というGemをインストールする。それにより、GitHub Pagesで用いられるGemがすべてインストールされる

Gemfileの書き方については、[Bundlerの公式ドキュメント][2]を参照してください。

`~> 232`の部分は執筆時点（2026年2月13日）でのGitHub Pagesで使える`github-pages`のバージョンです。
このバージョンは[GitHub Pagesの公式ページ][3]で確認してください。

Gemをインストールします。

```
$ bundle install
```

`bundle`はBundlerを起動するコマンドです。

`Bundle complete!`というメッセージが出れば、Gemのインストールは成功しています。
インストールされたGemの中に`jekyll 3.10.0`も含まれています。
それを確かめるために、`jekyll -v`でバージョンを確認しましょう。
BundlerでインストールしたGemを起動するには、かならず`bundle exec`をつけて起動しなければなりません。

```
$ bundle exec jekyll -v
jekyll 3.10.0
$ ls
Gemfile  Gemfile.lock  mise.toml
$
```

ディレクトリの中に、新たに`Gemfile.lock`というファイルができています。
このファイルには、インストールしたGemの一覧が記録されています。

## Jekyllソースのテンプレートの作成

Jekyllのソースファイルのテンプレートを作りましょう。
Jekyllの`new`コマンドを用います。
一旦`temp`という一時ディレクトリにテンプレートを作成し、その中の必要ファイルをコピーします。

```
$ bundle exec jekyll new temp --skip-bundle
$ ls temp
404.html  Gemfile  _config.yml  _posts  about.md  index.md
```
コピー対象は、`Gemfile`以外の5つです。
`cp`コマンドの最後にカレントディレクトリを表すドット（`.`）があることに注意してください。
また、`_posts`ディレクトリ内のすべてのファイルもコピーするために`-r`オプションが必要です。
最後に要らなくなった`temp`ディレクトリを削除します。

```
$ cp -r temp/404.html temp/_config.yml temp/_posts temp/about.md temp/index.md .
$ rm -rf temp
$ ls
404.html  Gemfile  Gemfile.lock  _config.yml  _posts  about.md  index.md  mise.toml
$ 
```

このディレクトリ`my-blog`は、Jekyllのソースコードのひな型ファイルを含んでおり、「ソースディレクトリ」と呼ばれます。
また、すべてのソースの最上位にあるディレクトリ、という意味で「ルートディレクトリ」といいます。
Linuxの「ルート」と混同する恐れがあるときは、「Jekyllのルートディレクトリ」などと表現するのが良いでしょう。

## ローカル・サイトのビルドとローカル・サーバの起動

次に、ローカル上で、サイトをビルドし、ローカル・サーバを起動します。

```
$ bundle exec jekyll serve
```

このウェブページをブラウザで見ることができます。
ブラウザを起動し、[`localhost:4000`][4]を開きます。
すると、次のような画面が現れるはずです。

![My-blog-index]({{ "/assets/images/my-blog-index.png" | relative_url }})

このページには「About」「Welcome to Jekyll」などのリンクがあるので、クリックして画面の遷移を試してみてください。

サーバーを停止するには、端末から`CTRL+C`を入力します。

ここまでで、駆け足ですが、Jekyllのひな型を作成してブラウザで表示させるまでを見てきました。
次のセクションでは、`my-blog`ディレクトリの中を見ながら、Jekyllが何をしているのかを調べていきます。

[1]: https://mise.jdx.dev/
[2]: https://bundler.io/guides/gemfile.html
[3]: https://pages.github.com/versions.json
[4]: http://localhost:4000/