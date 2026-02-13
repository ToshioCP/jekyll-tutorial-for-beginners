---
layout: section
title: Git
description: Gitの使い方
chap: 2
sec: 8
---

第2章ではGitHub Pagesを用いて、第1章で作成したサイトを公開することを目指します。
このセクションでは、それに必要なGitの使い方を学びます。

## Gitについて

ソフトウェアの開発では、機能の拡充、バグの対策など、様々な改良が日々行われます。
第1章で作成したJekyllのソースファイルでも書き直しが行われましたし、これからも行われるでしょう。
それを記録するのがバージョン管理システムで、その代表格がGitです。

Gitの対象となるディレクトリとその履歴情報をレポジトリといいます。
このセクションでは、`my-blog`のソースファイルをGitで管理する実例を通して、Gitの基本的使い方を学びます。

## Gitのインストールと初期設定

Linuxディストリビューションには、Gitのパッケージが用意されています。
Ubuntuならば、

```
$ sudo apt install git
```

で簡単にインストールできます。

Gitにユーザ名とメールアドレスを登録します。

```
$ git config --global user.name "ユーザー名"
$ git config --global user.email "メールアドレス"
```

ユーザ名に空白がある場合、例えば「Taro Yamada」のような場合、ダブルクォートで囲む必要があります。
これは、Bashが空白を引数の区切りと解釈するためです。

Gitのユーザ名、メールアドレスとGitHubのユーザ名、メールアドレスは別物ですが、同じにすることをお勧めします。
また、Gitに登録したユーザ名とメールアドレスはGitHubを通して公開されるということに注意してください。

次に、Gitで使うエディタを設定します。

```
$ git config --global core.editor "エディタ名"
```

エディタにVS Codeを指定する場合は`--wait`オプションをつけてください。

```
$ git config --global core.editor "code --wait"
```

設定をしない場合は、デフォルトのエディタ（Ubuntuではnano）になります。

GitHubにおけるメイン・ブランチのデフォルト名は`main`です。
それにあわせて、Gitのデフォルト・ブランチ名を`main`に設定します。

```
git config --global init.defaultBranch main
```

この設定は、次に述べる`git init`よりも前に行ってください。

## レポジトリの初期化

カレントディレクトリを、`my-blog`ディレクトリに移動します。
このディレクトリをGitのレポジトリにするには、initコマンドを使います。

```
$ git init
$ ls -a
.  ..  .git  .gitignore  .ruby-version  404.html  Gemfile  Gemfile.lock  _config.yml  _posts  _site  about.md  assets  index.md  mise.toml
```

新たに`.git`というディレクトリが作成されていることがわかります。
このディレクトリにバージョンの履歴情報が保存され、その管理をGitが行うのです。

### 作業ディレクトリの状態確認

作業ディレクトリとはGitの管理対象であるディレクトリのことで、ワーキング・ツリーとも言います。
ここでは、`my-blog`ディレクトリが作業ディレクトリにあたります。

作業ディレクトリの状態を確認するには次のようにタイプします。

```
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .ruby-version
        404.html
        Gemfile
        Gemfile.lock
        _config.yml
        _posts/
        about.md
        assets/
        index.md
        mise.toml
```

- 「On branch main」は、Git操作の対象となっているのが「main」（というブランチ）だということを示している。
- 「No commits yet」は、「まだコミットがありません」という意味だが、コミットについてはのちほど説明。
- 「Untracked files」はトラッキング（追跡）されていないファイル、つまりGitの履歴に書かれていないファイル、ということ。

Gitの表示メッセージは良く読むようにしましょう。
そこには、次に何をしたらよいかが書かれていることが少なくありません。
ここでは、次のようなことが書かれています。

```
  (use "git add <file>..." to include in what will be committed)
```

後にコミットされるファイルを`git add <file>...`を使って（ステージング・エリアに）含めましょう、という意味です。
まだステージング・エリアの説明はしていませんが、とりあえずこのようなメッセージが出たら、それに従っておいても大きな間違いにはならないことが多いです。

## .gitignore

ところで、`git add`を使う前に、履歴に残すべきファイルと残すべきでないファイルについて考えてみましょう。

```
$ ls -a
.  ..  .git  .gitignore  .ruby-version  404.html  Gemfile  Gemfile.lock  _config.yml  _posts  _site  about.md  assets  index.md  mise.toml
```

この中で履歴に残す必要のないのは、`_site`です。
なぜなら、その内容はJekyllがソースから生成したものだからです。
そして、このレポジトリをクローンしたユーザーは`_site`をいつでもJekyllで生成できます。

このように、生成物については履歴に残す必要がない、というのがひとつの原則です。

履歴に記録しないファイルは「.gitignore」というファイルに記述します。
それにより、Gitはそれらのファイルを記録対象から外します。

ところで、`.gitignore`はすでにディレクトリ内にありました。
これは、Jekyllがnewコマンドで作成したものです。
中身を確認してみましょう。
catコマンドを使うとファイルの中身を表示できます。

```gitignore
$ cat .gitignore
_site
.sass-cache
.jekyll-metadata
```

Jekyll自身が作ったものだけあって、`_site`はきちんと入っていましたね。
`.sass-cache`と`.jekyll-metadata`はディレクトリ内にはありませんが、入れておいて良いでしょう。

さきほど`git status`したときに、履歴に残すべきファイルのリストに`_site`がありませんでした。
それは、`_site`が`.gitignore`に書かれていたからなのです。

## git add

Gitでは、ファイルを直接履歴に書くことはできません。
その中間にステージング・エリアがあり、それを通して履歴に書き込みます。

- ファイルやディレクトリをステージング・エリアに登録
- ステージング・エリア全体を履歴に登録

第1段階のステージング・エリアにファイルやディレクトリを登録するのが`git add`コマンドです。
全てのファイルをaddするには、カレントディレクトリ「.」をaddすれば良いので、

```
$ git add .
```

とタイプします。
最後にピリオド（カレントディレクトリを指す）があることに注意してください。

```
$ git add .
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .gitignore
        new file:   .ruby-version
        new file:   404.html
        new file:   Gemfile
        new file:   Gemfile.lock
        new file:   _config.yml
        new file:   _posts/2026-01-30-st-george-church.md
        new file:   _posts/2026-01-31-kapitan-keling-mosque.md
        new file:   about.md
        new file:   assets/images/DimSum.jpg
        new file:   assets/images/KapitanKelingMosque.jpg
        new file:   assets/images/StGeorge.jpg
        new file:   assets/images/StreetArt.jpg
        new file:   index.md
        new file:   mise.toml

$
 ```

記録すべきファイルがすべて「Changes to be committed（コミット予定の変更点）」に書かれています。
このことは、表示されているファイルがステージング・エリアに登録されたということを意味します。
なお、ステージング・エリアに誤ってファイルを入れてしまった場合はアンステージ（ステージング・エリアから除くこと）できますが、それもGitの出力の中に書かれています。

```
  (use "git rm --cached <file>..." to unstage)
```

## git commit

ステージング・エリアのファイルを記録に残すことをコミットといいます。
英語のコミット（commit）にはいろいろな意味がありますが、その中に「（物事を）（記憶・焼却など）にゆだねる、付す」という意味があります。

commit an idea to writing => 着想を書き留める

`git commit`コマンドでファイルを記録し、その記録に関するメモを残すことができます。
コマンドを発行すると、エディタが立ち上がるので、短くメモを書きます。
例えば「Add first draft of blog」などです。
コミットは一般には英語で書くことが多いです。

コミットするとメッセージが表示されます。

```
$ git commit
[main (root-commit) 9c39a64] Add first draft of blog
 15 files changed, 466 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .ruby-version
 create mode 100644 404.html
 create mode 100644 Gemfile
 create mode 100644 Gemfile.lock
 create mode 100644 _config.yml
 create mode 100644 _posts/2026-01-30-st-george-church.md
 create mode 100644 _posts/2026-01-31-kapitan-keling-mosque.md
 create mode 100644 about.md
 create mode 100644 assets/images/DimSum.jpg
 create mode 100644 assets/images/KapitanKelingMosque.jpg
 create mode 100644 assets/images/StGeorge.jpg
 create mode 100644 assets/images/StreetArt.jpg
 create mode 100644 index.md
 create mode 100644 mise.toml
$ 
```

- mainブランチの9c39a64（から始まる）IDに「Add first draft of blog」というメッセージをつけて保存した
- ファイルが15個変更され（この場合は新規登録され）、466行が追加された
- 新規ファイルがモード100644（ノーマルなファイル、オーナーのみ読み書き可、グループと一般ユーザは読みのみ可）で記録された

このように、Gitにはファイルの内容だけでなくモード（100644）も記録されます。

（注）Gitのファイルモードについては、Gitリファレンスの[git-fast-import](https://git-scm.com/docs/git-fast-import)を参照してください。

```
$ git status
On branch main
nothing to commit, working tree clean
$
```

Git の状態は、mainブランチで、コミットすべきものはなく、作業ディレクトリ（working tree）はクリーン（履歴の最後の状態と同じ）です。

## 参考リンク

最後にGitの参考ページのリンクを付けておきます。

- [Gitのウェブサイト](https://git-scm.com/)
- [Pro Git](https://git-scm.com/book/en/v2)・・・日本語を選択できます。上級者向け
- [サル先生のGit入門](https://backlog.com/ja/git-tutorial/)・・・初級者向け
