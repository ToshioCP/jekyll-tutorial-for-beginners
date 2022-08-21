---
layout: document
title: Gitの使い方
description: Gitの良く使われるコマンドを解説
chap: 7
---

サイトができあがったらGitHubにアップロードして公開します。
アップロードをGitではプッシュといいます。

プッシュするときによく使われる一連の手続きは次のようなものです。

- git status
- git add
- git commit
- git push

これらについて見ていきましょう。

## ワークツリーの状態確認

ワークツリーが前回のコミットからどう変更されたかを確認するには「git status」を使います。

```
$ git status
ブランチ main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   _config.yml

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)
        .jekyll-cache/
        Gemfile
        Gemfile.lock
        _site/
        index.md

no changes added to commit (use "git add" and/or "git commit -a")
```

これはあくまで一例で、あなたのローカルで示される内容とは異なると思います。

いろいろ表示されていますが、ここで注目したいのは「Changes not staged for commit（コミットのためにステージされていない変更）」と「追跡されていないファイル」のところにあるファイルです。
Gitはバージョン管理ツールで、最後にコミットされた時点までの情報を隠れディレクトリ「.git」に保存しています。
その記録と現在のディレクトリを比較してこのようなメッセージを表示しています。

この例ではGitHubからのクローン以後、一度もコミットしていません。

クローン元のGitHubのレポジトリではどのようなコミットをしていたでしょうか？

- README.mdを作成した。これが最初のコミット
- 「jekyll-theme-leap-day」のテーマをセットした。
これは設定画面でテーマを設定したときにGitHub側で（自動的に）コミット

クローンした後、

- Gemfileを追加した
- 「bundle install」でGemfile.lockが追加された
- \_config.ymlを変更した
- index.mdを追加した
- Jekyllを起動したときに\_siteと.jekyll-cacheディレクトリができた

これらが「git status」で表示されているファイルなのです。

ここまでの仕事をGitを使って記録しましょう。
ここで考えなければならないのは、すべて記録しておくべきだろうか、ということです。

- Gemfileは必要なgemの記述だから記録する
- Gemfile.lockはGemfileのgemだけでなく、実行に必要なすべてのgemとそのバージョンを記述しているので記録する
- \_config.ymlは記録する
- index.mdは記録する
- \_siteと.jekyll-cacheはJekyllが生成したフォルダで、Jekyllの起動に必要なわけではない。これは記録しない。

## .gitignore

記録しないファイルは「.gitignore」というファイルに記述します。
それにより、Gitはそれらのファイルを記録対象から外します。
エディタで次のように書き、「.gitignore」という名前で保存します（ファイル名の先頭のドット（`.`）を忘れないようにしてください）。

```gitignore
# Cache directory
.jekyll-cache

# HTML directory created by Jekyll
_site
```

一般的にGitで記録すべきでないファイルは

- 生成されたファイル、生成途中で作られる中間ファイル
- テキストファイルのバックアップファイル。たとえば「index.md~」のようにティルダのつくもの。

です。
再度「git status」してみましょう。

```
$ git status
ブランチ main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   _config.yml

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        Gemfile
        Gemfile.lock
        index.md

no changes added to commit (use "git add" and/or "git commit -a")
```

.gitignoreに書かれたファイルが表示されなくなりました。

## git add

ここに表示されたファイルは記録に残すようにします。
そのための手順がさきほどの画面に出ています。

```
  (use "git add <file>..." to update what will be committed)
  (use "git add <file>..." to include in what will be committed)
```

この2行から、「git add &lt;file>」とコマンド入力することによって、アップデート（変更）とインクルード（追加）ができる、ということが分かります。
全てのファイルをaddするには、カレントディレクトリ「.」をaddすれば良いので、

```
$ git add .
```

とタイプします。
このコマンドで変更されたファイルと追加されたファイルがステージの状態になります。
この状態ではまだGitに記録されたわけではありません。
記録の準備の段階が完了したということです。
再び状況確認します。

```
$ git status
ブランチ main
Your branch is up to date with 'origin/main'.

コミット予定の変更点:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitignore
        new file:   Gemfile
        new file:   Gemfile.lock
        modified:   _config.yml
        new file:   index.md
```

記録すべきファイルがすべて「コミット予定の変更点」に書かれています。
このことは、表示されているファイルがステージに登録されたということを意味します。
なお、ステージに誤ってファイルを入れてしまった場合はアンステージ（ステージから除くこと）できます。

```
  (use "git restore --staged <file>..." to unstage)
```

「git restore \-\-staged &lt;file>」でアンステージできると書かれています。
このように、Gitは親切で、そのときに使えるコマンド（の主なもの）が表示されます。

## git commit

ステージされたファイルを記録に残すことをコミットといいます。
英語のコミット（commit）にはいろいろな意味がありますが、その中に「（物事を）（記憶・焼却など）にゆだねる、付す」という意味があります。

commit an idea to writing => 着想を書き留める

「git commit」コマンドでファイルを記録し、メモを残すことができます。
コマンドを発行すると、エディタが立ち上がるので、短くメモを書きます。
例えば「Add Gemfile and index.md.」などです。
上書き保存します。
上書き保存後に下のようにメッセージが表示されます。

```
$ git commit
[main a516afb] Add Gemfile and index.md.
 5 files changed, 103 insertions(+), 1 deletion(-)
 create mode 100644 .gitignore
 create mode 100644 Gemfile
 create mode 100644 Gemfile.lock
 create mode 100644 index.md
```

- mainブランチのa516afb（から始まる）IDに「Add Gemfile and index.md.」というメッセージをつけて保存した
- ファイルが5つ変更され、103行が追加され1行が削除された（実際には\_config.ymlで4行追加したのですが、1行削除されて5行追加されたようにgitが認識しています）。
- 新規ファイルがモード100644（オーナーのみ読み書き可、グループと一般ユーザは読みのみ可）で記録された

このように、Gitにはファイルの内容だけでなくモードも記録されます。

```
$ git status
ブランチ main
このブランチは 'origin/main' よりも1コミット進んでいます。
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Git の状態は、ローカルがorigin/main（リモート・レポジトリ＝GitHubのレポジトリ）よりも1コミット進んでいる、と表示されました。
先程のコミットの分だけローカルのほうが進んでいるわけです。
ワークツリーは（最後のコミットから）特に変更無くクリーンでコミットするようなものはない、と表示されています。

## git push

さきほどの表示に

```
  (use "git push" to publish your local commits)
```

とありました。
「git push」を使ってローカルのコミットを（GitHubに）公開できる。
ということです。
ローカルに対してGitHubはリモートといいます。
ローカルからリモートに新たなコミットをコピーするのが「git push」です。
逆にリモートが進んでいるときは「git pull」を使ってローカルに取り込みます。
大事なことは、コミットしたものだけがプッシュされることです。
コミットしていないワークツリーはプッシュしないので、必ずコミットしてください。

GitHubにpushするには、いくつかの方法がありますが、ここではhttpsとパーソナル・アクセス・トークン（PAT）を使うことにします。
PATはパスワードのようなものですが、パスワードよりは強力なものです。

- GitHubを開く
- 右上のアイコンをクリック＝＞ポップアップメニューからSettingsをクリック
- 左のメニューの一番下「Developer settings」をクリック＝＞「personal access tokens」をクリック
- 「Generate new token」をクリック＝＞パスワードを入力する＝＞以下画面に従ってトークンを生成する
- 生成されたトークンをメモしておく。またはエディタでファイルに保存しておく（ファイル保存をお勧めします）。

トークンは以後毎回pushで使えます。
「git push」すると、ユーザ名とパスワードを要求されます。
パスワードにはPATを入力します。

```
$ git push
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 8 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 1.50 KiB | 1.50 MiB/s, done.
Total 7 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ToshioCP/jekyll-tutorial-for-beginners.git
   4901be1..a516afb  main -> main
```

プッシュで気をつけなければならないことがあります。
それは、一度プッシュしたものを訂正したり削除したりしてはいけないということです。
なぜなら、その訂正や削除の前にクローンやプルしたユーザのレポジトリが、公開レポジトリと異なってしまうからです。
そのユーザは、以後の公開レポジトリの変更をプルできなくなってしまいます。

これを避けるには、一度プッシュした内容の上に、訂正を改めてプッシュすることです。
こういうことにならないよう、プッシュの前には一度冷静に内容確認をしましょう。

なお、プッシュ以前のローカルのコミットは変更、削除して構いません。
その方法については、この章の最後にあるリンクを参照してください。

## ローカルでテスト、リモートで公開

ここまでで、ローカルとリモートを同じ状態に保つことができるようになり、しかもローカルでJekyllを使ってテストできるようになりました。
今後は

- ローカルでウェブサイトの内容を作っていく
- ローカルでJekyllを使ってテストする
- コミット
- GitHubにプッシュ＝＞公開

という手順をくりかえしながら開発をすることになります。

最後にGitの参考ページのリンクを付けておきます。

- [Gitのウェブサイト](https://git-scm.com/)
- [Git Pro](https://git-scm.com/book/en/v2)・・・日本語を選択できます。上級者向け
- [サル先生のGit入門](https://backlog.com/ja/git-tutorial/)・・・初級者向け
