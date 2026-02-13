---
layout: section
title: GitHubにレポジトリをプッシュ
description: ローカルに構築したGitレポジトリをGitHubにプッシュする方法の説明
chap: 2
sec: 10
---


この章ではGitHubのレポジトリを作成し、すでに作成した`my-blog`をアップロードします。
そして、GitHub Pagesの設定をして、インターネット上にブログを公開できるようにします。

## GitHubアカウントの作成

アカウントを持っていない方は、まずアカウントを作成します。

- ブラウザでGitHub（<http://github.com/>）にアクセスし、画面右上の「サインアップ」（英語表示の場合は「Sign up」）ボタンをクリック
- 以下、ウェブサイトの案内に従う

![GitHubのスタート画面]({{ "/assets/images/github.png" | relative_url }})

以下の作業はGitHubにサインイン（サインインとログインは同じです）した状態で行います。

## GitHubレポジトリの作成

- GitHubにログインする
- 右上に並んだボタンの中の`+`と書かれたボタンをクリックする
- `Create a new repository`画面になる
- レポジトリ名（Repository name）は`my-blog`とする。ローカル・レポジトリのディレクトリ名と同じ名前が良い
- 説明（description）は「Blog about Penang Island」（ペナン島についてのブログ）とする
- `Choose visibility`は`Public`（公開）とする。GitHub Pagesを作成する場合は公開にしなければならない
- `Start with a template`は`No template`
- `Add README`は`Off`
- `Add .gitignore`は`No .gitignore`
- `Add license`は`No license`
- `Create repository`ボタンをクリックする

レポジトリが作成されます。
画面には、次にすべきこと（複数の方法あり）について説明があります。
最下段にある説明が今回のケースに当てはまりますが、いきなりこの説明のようにやらずに、次のサブセクション以降の方法をとってください。

```
…or push an existing repository from the command line
git remote add origin git@github.com:ToshioCP/my-blog.git
git branch -M main
git push -u origin main
```
## README.mdの作成

いったんここでLinuxの端末に戻ります。
まずは、カレントディレクトリを`my-blog`に移動します。

GitHubに`my-blog`ローカル・レポジトリを上げる（アップロードすること）前に、いくつかやらなければなりません。

- `README.md`の作成
- `_config.yml`の変更

まず、README.mdについて説明します。
このファイルはレポジトリについての説明ファイルで、GitHub上では、コード一覧画面（最初に表示される画面）に現れます。
以下の例のような内容のテキストファイルを作り、`README.md`という名前でローカル・レポジトリのトップに保存してください。
ただし、`<username>`は、あなたのGitHubのユーザ名に置き換えてください。

```markdown
このレポジトリには「ペナン島へ行こう」というタイトルのブログのソースが収められています。
ブログはJekyllとGitHub Pagesの仕組みによって、次のアドレスで見ることができます。

[https://<username>.github.io/my-blog/](https://<username>.github.io/my-blog/)

このサイトをローカルで見る方法は以下の通りです。

1. Gitでこのレポジトリをクローンする
2. そのローカルレポジトリのトップにカレントディレクトリを移動する
3. 端末から`bundle install`
4. 端末から`bundle exec jekyll serve`
5. ブラウザで`localhost:4000/my-blog`にアクセスする

以上でローカルでブログを見ることができます。
```

## _config.ymlの設定

Jekyllの設定ファイル`_config.yml`を変更します。
変更は以下のようにしてください。これをコピペしても構いません。
なお、この例における、`author`, `email`に対する値は、架空のものです。
また、`<username>`のところは、GitHubに登録したユーザ名です。

```yaml
# Site settings
title: ペナン島へ行こう
author: 檳城島人
email: shimato-binjo@example.com
description: >-
  ペナン島には世界歴史遺産のジョージタウンをはじめ、魅力ある観光スポットが点在しています。
  このサイトでは、ペナン島の魅力を伝えるための情報を発信します。
domain: "<username>.github.io"
url: "https://<username>.github.io"
baseurl: "/my-blog"
github_username: <username>
lang: ja

# Build settings
# ---- GitHub Pages requires the following settings ----
lsi: false
safe: true
incremental: false
highlighter: rouge
gist:
  noscript: false
kramdown:
  math_engine: mathjax
  syntax_highlighter: rouge
# ------------------------------------------------------
plugins:
  - jekyll-feed

# Minima theme settings
theme: minima
minima:
  date_format: "%Y年%m月%d日"
show_excerpts: true

# Exclude from processing
exclude:
  - Gemfile
  - Gemfile.lock
  - .git
  - .gitignore
  - README.md
  - mise.toml
  - .ruby-version
```

上の設定について簡単に解説します。
以下では、一行に収めるために、マッピングを`{`と`}`で囲んだスタイル、シーケンス（リスト）を`[`と`]`で囲んだスタイルで書きました。

### 基本情報の設定

- `title`: ウェブサイトのタイトル
- `author`: ウェブサイトの作成者
- `email`: 連絡先電子メール。`email`の設定をしなければ（行全体を削除）、電子メール非公開となる
- `description`: ウェブサイトの説明

現代はスパムメールなどが多く、電子メールの公開はリスクがあります。
非公開を選ぶ場合は、上に書いたように`email`の行全体を削除してください。

### URLの設定

GitHub PagesのURLに一致するように設定します。
先ほども書きましたが、`<username>`のところは、GitHubに登録したあなたのユーザ名です。

- `domain`: ドメイン名。GitHub Pagesでは**`<username>.github.io`**
- `url`: サーバーのURL。GitHub Pagesでは**`https://<username>.github.io"`**
- `baseurl`: ベースURL。GitHub Pagesでは、**レポジトリ名とベースURLを一致させる**
- `github_username`: GitHubのレポジトリ所有者名。GitHubに登録したユーザ名

### 言語の設定

- `lang: ja` langは言語（language）のことで、値の書き方は決まっています。日本語は`ja`と書きます。

### GitHub Pagesの要求する設定

[GitHub Pagesのドキュメント][1]には、`lsi`, `safe`, `incremental`, `hilighter`, `gist`, `kramdown`の設定について書かれています。
以下の設定は、やや専門的なので、はじめてJekyllを使う人には理解しにくいと思います。
とりあえず、例のように設定しておけば良いでしょう。
以下は簡単な説明ですが、読み飛ばしても構いません。

- `lsi: false` 「Latent Semantic Indexing」（潜在的意味インデックス）
- `safe: true` カスタムプラグインを無効化
- `incremental: false` 変更があったファイルだけを再構築して、ビルド時間を短縮
- `highlighter: rouge` シンタックスハイライト（色付け）にRougeを使う
- `gist: {noscript: false}` gistの展開において、noscriptタグを出力しない
- `kramdown: { math_engine: mathjax, syntax_highlighter: rouge }`。Markdownの変換において、数式の処理は「MathJax」、ハイライトは「Rouge」を使う
- `plugins: [jekyll-feed]` RSSフィード（購読用の更新通知ファイル）を自動生成するプラグインを有効にする

### プラグインに関する設定

- `plugins: [jekyll-feed]` jekyll-feedプラグインを使う

### テーマ（Minima）に関する設定

- `theme: minima` テーマをMinimaに設定
- `minima: {date_format: "%Y年%m月%d日"}` 年月日の表現を日本式（2026年2月2日の形式）で表示
- `show_excerpts: true` ブログ記事のリストに本文からの抜粋を加える

### ビルドから除くファイルの設定

`exclude`キーでは、ビルドから除くファイルを指定します。
指定されたファイルについて簡単に説明しますが、ここは読み飛ばしても構いません。

- `Gemfile`: Rubyライブラリ（Gem）を記述
- `Gemfile.lock`: Gemfileに基づきBundlerがインストールするGemの一覧
- `.git`: Gitの履歴などの保存ディレクトリ
- `.gitignore`: Gitが履歴に保存しないファイルを設定する
- `README.md`: GitHubのREADMEファイル
- `mise.toml`: Rubyのバージョン指定
- `.ruby-version`: Rubyのバージョン指定

### 大域的な定数としての利用

上記の例ではでてきませんでしたが、`_config.yml`には、すべてのページから参照できる 「グローバル定数」を定義できます。
Liquidテンプレート内では、`site` というキーワードを使ってアクセスします。

例えば、`_config.yml`が次の記述を含んでいるとします。

```yaml
doc-GitHub: "https://docs.github.com/en/pages"
doc-Jekyll: "https://devdocs.io/jekyll~3/"
doc-Liquid: "https://shopify.github.io/liquid/"
```

これらのキーは、どのページ（例えばindex.mdやブログ記事）からでもLiquidを使って参照できます。
その際は`site.`の後にキーをつなげるだけです。

{% raw %}
```
各ドキュメントは次のリンクをクリックしてみることができます。

- [GitHub Pagesのドキュメント][1]
- [Jekyllのドキュメント][2]
- [Liquidのドキュメント][3]

[1]: {{site.doc-GitHub}}
[2]: {{site.doc-Jekyll}}
[3]: {{site.doc-Liquid}}
```
{% endraw %}

### 重要なルール：サーバの再起動

Jekyllのローカルサーバー（`jekyll serve`）は、ソースファイルの変更を即座に検知してブラウザに反映してくれますが、`_config.yml` の変更は反映されません。
Jekyllが`_config.yml`を読み込むのは、ビルド開始時のみです。
このファイルの書き換えた内容を反映させるためには、以下の手順が必要です。

1. ターミナルで `Ctrl + C` を押して、サーバーを停止する
2. 再度 `bundle exec jekyll serve` コマンドを実行してサーバーを再起動する

それでは、あらためて、ローカル・サーバを起動してみましょう。

```
$ bundle exec jekyll serve
```

`_config.yml`にベースURLを追加したので、ブラウザに入力するアドレスも変わります。

```
http://localhost:4000/my-blog
```

このように、ベースURLを付け加えることをわすれないでください。

## 変更をコミット

`README.md`の追加と`_config.yml`の変更をGitレポジトリにコミットし、履歴に加えます。
コミットで`-m`オプションをつけると、エディタで編集しなくてもコミットメッセージを記録できます。

```
$ git add .
$ git commit -m "Add README.md and change _config.yml"
```

## ローカル・レポジトリとGitHubの接続設定

すでに、`my-blog`は`git init`してGitのレポジトリになっています。
このレポジトリをさきほど作ったGitHubの`my-blog`レポジトリと結びつけます。

```
$ git remote add origin git@github.com:（ユーザー名）/my-blog.git
```

（ユーザ名）のところは、GitHubに登録したあなたのユーザ名を当てはめます。
これで、`origin`という名前のリモートレポジトリにGitHubのレポジトリが結びつきました。

結びつきができたら、ローカルの内容をGitHubにアップロード（push）します。
パスフレーズの入力が求められるので、sshの秘密鍵を作った時のパスフレーズを入力します。

```
$ git push -u origin main
Enter passphrase for key '/home/（Linuxのユーザ名）/.ssh/id_ed25519': 
Enumerating objects: 20, done.
Counting objects: 100% (20/20), done.
Delta compression using up to 8 threads
Compressing objects: 100% (16/16), done.
Writing objects: 100% (20/20), 6.82 MiB | 3.11 MiB/s, done.
Total 20 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:ToshioCP/my-blog.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
$ 
```

このコマンドはローカルの内容をリモートにアップロード（push）するものです。
`-u origin main`をつけることで、ローカルとリモートのmainブランチが結びつきます。
これにより、次回からは`git push`と打つだけで、自動的にここに送られるようになります

GitHubのレポジトリをブラウザで見ると、ローカルと同じファイルが表示されていることがわかります。

## GitHub Pagesの設定

GitHubの`my-blog`レポジトリを開きます。
以下の手順でGitHub Pagesの設定をします。

- 上方に並んだボタンの一番右にある`Settings`ボタンをクリック
- 左サイドバーにある`Pages`をクリック
- `Build and deployment`のセクションの`Source`は`Deploy from a branch`のままで良い
- `Branch`のところは、`None`のボタンをクリックし、`main`と`/(root)`を選び、`Save`をクリックして確定
- `Custom domain`以下は設定しなくて良い

しばらくしてから（1分程度）、そのページをリロードすると、一番上の`GitHub Pages`セクションにメッセージとボタンが現れます。

- `Your site is live at https://（ユーザ名）.github.io/my-blog/`。このメッセージは`my-blog`サイトのURLを示している
- `Visit site`のボタンをクリックするとそのページに飛ぶ

`Visit site`ボタンをクリックしてサイトに移動します。
すると、ローカルで`bundle exec jekyll serve`により見たページと同じものがネット上で確認できます。

[1]: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll#configuring-jekyll-in-your-github-pages-site