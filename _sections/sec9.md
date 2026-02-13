---
layout: section
title: GitをSSHで使うための準備
description: GitHubにSSHでアクセスするための準備として公開鍵をGitHubに登録する
chap: 2
sec: 9
---

GitをSSH経由で使う方法を説明します。
SSHの最初の設定は面倒ですが、一度設定してしまえば次からの接続は楽になります。

## この記事を読む前に注意していただきたいこと

会社のコンピュータなどの場合、インターネットと社内ネットワークの間にプロキシサーバーが置かれている場合が多いです。
プロキシサーバーの中にはSSHの通信を禁止している場合があります。
具体的には、SSHサーバーはポート番号22を使うので、プロキシがポート22への通信を破棄すればSSH通信を禁止できます。
この場合はポート22でGitHubへのSSH接続はできませんが、ポート443（https用のポート）でもGitHubにSSH接続できます。

[Using SSH over the HTTPS port][1]

以下では、プロキシがない場合（個人でプロバイダーを通してインターネットに接続する場合などで、22番ポートで接続する場合）を説明します。

## SSHについて

SSHは、2台のコンピュータがネットワークを通して安全にデータを通信するためのプロトコルです。
主にサーバにリモート・ログインして使うのに用いられますが、GitHubとGitの通信でも使えます。

SSHでは、最初にサーバー側とクライアント側がそれぞれ相手を確認する仕組みがあります。
それを「認証」、または英語で「オーセンティケーション（Authentication）」といいます。
認証には「公開鍵暗号方式」を使います。
公開鍵暗号方式については[ウィキペディア][2]に説明があります。

1. 「鍵」とは、暗号化や復号に使う**長い数字のデータ**のこと
2. 公開鍵暗号方式では、公開鍵と秘密鍵という2つの鍵を使う。公開鍵は相手側にわたし、秘密鍵は自分だけが持つ

ここでは、認証の理論的詳細には立ち入らず、具体的な設定のみを説明します。
また、[GitHubのドキュメント][3]にも、SSHを使ったGitとGitHubの通信の設定方法が書かれていますので参考にしてください。

## 鍵の生成

鍵の生成には`ssh-keygen`というコマンドを使います。

```
$ ssh-keygen -t ed25519 -C "ユーザのメールアドレス"
```

ed25519は暗号化の方式です。
ウィキペディアの[エドワーズ曲線デジタル署名アルゴリズム][4]に説明があります。

```
Enter a file in which to save the key
```

というプロンプトに対しては単にエンターキーを押せば良いです。

```
Enter passphrase (empty for no passphrase)
```

というプロンプトに対してはパスフレーズ（自分が新たに作成）を入力します。
パスフレーズはパスワードのようなものです。
自分自身の秘密鍵にアクセスする時にパスフレーズの入力が求められます。
パスフレーズはかならず控えておいてください。

これで、`~/.ssh/`に公開鍵「id_ed25519.pub」と秘密鍵「id_ed25519」が生成されます。

ここで行った手続きの詳細については[OpenSSHのウェブサイト][5]を参照してください。

## GitHubへの公開鍵登録

GitHubのアカウントを持っていない方は、まずサインアップ（登録）してアカウントを作ってください。
サインアップは難しくありません。
画面の指示にしたがってください。

作成した「あなたのコンピュータの公開鍵」をGitHubに登録します。
GitHubにサインインした状態で以下の作業をします。
[GitHubのドキュメント][6]も参考にしてください。

- GitHubをブラウザで開きサインインする
- 右上のアイコン（プロフィールアイコン）をクリックしポップアップメニューから「Settings」をクリックする
- 左側の列のメニューから「SSH and GPG Keys」をクリックする
- 右側の一番上SSH Keysの「New SSH Key」ボタン（緑のボタン）をクリック
- タイトルフィールドには、キーのタイトル、例えば「〇〇のデスクトップマシン」のようなメモを入力する
- ローカルのコンピュータの公開鍵「id_ed25519.pub」をエディタで開き、キー（最初のssh-ed25519からメールアドレスまでのすべて）をクリップボードにコピーする。
**（重要な注意）秘密鍵と間違わないでください**
- ブラウザ画面のKeyのところにペーストする
- 「Add SSH Key」ボタンをクリックする

## GitからSSHでGitHubにアクセスする

SSHの通信の最初に、GitHubからホスト鍵（GitHub側が作成した公開鍵）が送られてきます。
そのホスト鍵がパソコン内に登録されていなければ、受信した鍵のフィンガープリントが画面に表示され、ユーザのチェックを促します。
ユーザは、GitHubのウェブサイトに公開されている[GitHubの公開鍵のフィンガープリント][7]と、パソコンに表示されたフィンガープリントが一致するかどうかをチェックします。
一致していれば「yes」を、一致していなければ「no」を入力してください。

「yes」を入力するとホスト鍵がパソコンに取り込まれます。
以後は公開鍵がパソコン内に登録されているので、フィンガープリントの確認は必要ありません（表示もされません）。

このことを確認するために[JekyllのGitHubレポジトリ―][8]をクローンしてみましょう。

```
$ git clone git@github.com:jekyll/jekyll.git
Cloning into 'jekyll'...
The authenticity of host 'github.com (20.27.177.113)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
remote: Enumerating objects: 146382, done.
remote: Counting objects: 100% (38/38), done.
remote: Compressing objects: 100% (33/33), done.
remote: Total 146382 (delta 15), reused 5 (delta 5), pack-reused 146344 (from 2)
Receiving objects: 100% (146382/146382), 68.41 MiB | 9.79 MiB/s, done.
Resolving deltas: 100% (78396/78396), done.
$
```

上から6行目の最後の「yes」は端末にキーボードから入力したものです。
フィンガープリントのチェックをして、大丈夫だったので、「yes」としたものです。

ローカルにクローンしたJekyllのレポジトリは、後ほど使いますので、削除しないでください。

以上でGitHubにSSHでアクセスできるようになりました。

後ほど説明することになりますが、GitHubにSSHでデータを送るとき（例えば`git push`）、秘密鍵へのアクセスのために、パスフレーズの入力を求められます。
秘密鍵作成時に設定したパスフレーズを入力してください。

[1]: https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port
[2]: https://ja.wikipedia.org/wiki/%E5%85%AC%E9%96%8B%E9%8D%B5%E6%9A%97%E5%8F%B7
[3]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh
[4]: https://ja.wikipedia.org/wiki/%E3%82%A8%E3%83%89%E3%83%AF%E3%83%BC%E3%82%BA%E6%9B%B2%E7%B7%9A%E3%83%87%E3%82%B8%E3%82%BF%E3%83%AB%E7%BD%B2%E5%90%8D%E3%82%A2%E3%83%AB%E3%82%B4%E3%83%AA%E3%82%BA%E3%83%A0
[5]: https://www.openssh.com/
[6]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
[7]: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints
[8]: https://github.com/jekyll/jekyll
