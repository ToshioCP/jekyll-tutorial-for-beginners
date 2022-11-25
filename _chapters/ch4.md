---
layout: chapter
title: GitHub pagesのローカル・テスト
description: GitHub pagesで作成したサイトをローカルでテストする
chap: 4
---

この章では、作成したGitHubのレポジトリをローカルにクローンして、Jekyllでテストすることを説明します。
あわせて、Gitについても説明します。
GitはJekyllに限らず、開発では必須のアプリです。

## Gitについて

GitHubは、Git（バージョン管理アプリ）＋Hub（中心、ネットワークの経由地）という2つの言葉の組み合わせです。
すなわち、Gitレポジトリを複数のユーザが共有するためのハブです。
したがって、GitHubにはGitが欠かせません。
まだ使ったことのない人は、この機会に使い方を覚えましょう。
すでにGitに慣れている人はGitの説明の部分を飛ばして構いません。

## Gitのインストール

LinuxではディストリビューションにGitのパッケージがあります。
Ubuntuならば、

```
$ sudo apt install git
```

で簡単にインストールできます。

Windowsの場合は[Gitのホームページ](https://git-scm.com/)から、「Downloads」をクリックしてダウンロードページに行き、Windows版のインストーラをダウンロードします。
あとはインストーラの指示に従ってインストールします。

## Git cloneする

レポジトリをクローンする方法を説明します。

### 初期設定

Gitにユーザ名とメールアドレスを登録します。

```
$ git config --global user.name "ユーザー名"
$ git config --global user.email "メールアドレス"
```

ユーザ名に空白がある場合、例えば「Taro Yamada」のような場合、ダブルクォートで囲む必要があります。
これは、Bashが空白を引数の区切りと解釈するためです。

Gitのユーザ名とGitHubのユーザ名は別物です。
その2つを違うものに設定することは可能ですが、同一にすることによって無用の混乱を避けるという考えもあると思います。
（ただこのことをシリアスに考える必要はありません。
実際、異なる名前を設定しているユーザは大勢います）

Gitのユーザ名とメールアドレスはコミットに記録されます（コミットは第7章で説明します）。
そのコミットがGitHubにプッシュされると、GitHubはこのメールアドレスでコミットをGitHubユーザにリンクし、そのGitHubユーザ名をコミッターにして表示します。
この機能が正しく行われるように、GitのメールアドレスはGitHubに登録したときのものと同じにしてください。

他のユーザがレポジトリをクローンすると、そのユーザはコミットを（ローカルのGitを使って）見ることができます。
そのコミットにはコミッターがGitに設定したユーザ名とメールアドレスも書かれています。
つまり、Gitに登録したユーザ名とメールアドレスはGitHubを通して公開されるということに注意してください。

Gitで使うエディタを設定することができます。

```
$ git config --global core.editor エディタ名
```

設定をしないとデフォルトのエディタ（Ubuntuではnano）が使われます。

### GitHubレポジトリのクローン

すでに作成したGitHub pagesのレポジトリをローカルにコピー（クローンという）しましょう。

- ブラウザで[GitHub](https://github.com/)にアクセスする。
サインイン（ログインのこと）していない場合は、「Sign in」をクリックして、サインインする。
- 画面右上のアイコンをクリックし、現れたポップアップメニューから「Your repositories」をクリックする
- 前回作成したレポジトリ名をクリックする

ここまでで、レポジトリが表示されます。
ここで、緑色のボタン「Code」をクリックし、ポップアップメニューの四角が斜めに重なっているアイコン（コピーを示すアイコン）をクリックします。
そのとき、「HTTPS」「SSH」「GitHub CLI」の3つのタブが現れますが、ここでは「HTTPS」を使います。

![clone]({{ "/assets/images/copy_icon.png" | relative_url }})

クリックしても何も目に見える変化はありませんが、実はパソコンのクリップボードにレポジトリのアドレスがコピーされています。

- 端末（Windowsではコマンドプロンプト、以下は「端末」と表示）から、「git clone （クリップボードをペースト）」と入力する。
クリップボードからペーストするには、Linuxでは「Ctrl+Shift+V」を、Windowsでは「Ctrl+V」を押す。
Windowsで上手く行かない場合は、コマンドプロンプトの設定ができていない可能性がある。
「コマンドプロンプトでコピペ」でグーグル検索するとやり方がヒットしますので、参照してください。
- エンターキーを押すと、レポジトリがクローンされる。

レポジトリ名と同じ名前のディレクトリが作成されます。
このディレクトリがレポジトリのクローンです。
ディレクトリを開くと「README.md」と「\_config.yml」が入っているはずです。

うまくクローンできたでしょうか？
良くわからない場合は「git clone」でネットを検索すると解説サイトが見つかると思います。

### レポジトリの状態確認

端末のカレントディレクトリをクローンしたレポジトリに移動します。
レポジトリの状態を確認するには次のようにタイプします。

```
$ git status
ブランチ main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

- 「status」は状態、状況のこと。
- ブランチの意味については、ここでは説明を省略する。
操作の対象となっているのが「main」（というブランチ）だということが、表示されている。
- 次の英語は「ブランチは'origin/main'（これはGitHubのレポジトリのこと）とともに、アップデートされています」という意味。
- 最後の行は「（新たに）コミットするようなものはなし。ワークツリーはクリーン（新たに加わったものはない）」ということ。
ワークツリーはこのディレクトリ（これを作業ディレクトリともいう）のこと。
例えば、このディレクトリに新しいファイルを作ることは「ワークツリーに新規ファイルを作成」することになる。

## Jekyllをローカルで使う

クローンしたレポジトリでJekyllを動かしてみましょう。
ローカルで動かすためにはいくつか準備が必要です。

### gethub-pages gem

ローカルではBundlerとGemfileを使います。
Gemfileにはそのレポジトリで使うRubyのライブラリを記述します。
Rubyではライブラリのことを「gem」（ジェム、英語で宝石のこと）といいます。
さらに、

- ライブラリ（gem）をまとめた[Rubygems](https://rubygems.org/)からgemをダウンロードするコマンドも「gem」。
例えば「`gem install jekyll`」とすると、jekyllをダウンロードできる。
bundlerというコマンドでgemをインストールすることも多いが、それはgemコマンドが必要ないということではない。
gemとbundlerは動作が違うので状況で使い分ける。
- Rubygemsとパッケージ管理の全体を指して「gem」ということもある。

Gemfileの記述では、必要なライブラリ名を「gem」の後に続けます。

```
source "https://rubygems.org"

gem "github-pages", "~> 227", group: :jekyll_plugins
gem "webrick"
```

- 最初の行はライブラリを[Rubygems](https://rubygems.org/)から取得するという意味。
- 「github-pages」は、GitHubと同じ環境を構築するためのgem。
- 「webrick」はJekyllがローカルでウェブサーバとして振る舞うために使われるライブラリ。
Ruby3.0以降ではWebrickが標準添付ではなくなったので、gemをインストールしなければならない。

ここで`github-pages`gemについて説明しておきます。

GitHubでは、レポジトリからページを作成する時にgemが使われます。
私達はローカルでページを構築していますが、もしJekyllなどのgemのバージョンがGitHubと違っていたらどうなるでしょうか？
異なるバージョンでは動作が異なる可能性がありますから、生成されるHTMLファイルにも違いができるかもしれません。
これはローカルの実行がGitHubのテストにはならないということを意味します。
ですから、ローカルのgemとGitHubのgemのバージョンは全て一致させなければなりません。

ところで、GitHubで使うgemのバージョンは公開されています

<https://pages.github.com/versions/>

たくさんのgemが含まれていますね。
これだけのgemについて人手でチェックするのは大変です。
それを解決するgemが`github-pages`です。
このgemをGemfileに書いておけば、必要なgemを正しいバージョンで取り込んでくれるのです。

```ruby
gem "github-pages", "~> 227", group: :jekyll_plugins
```

gemメソッドの2番めの引数「~> 227」は、先程のGitHubのgemバージョン一覧に載っていた`github-pages`のバージョンです。
「~>」は「メジャーバージョンは同じで、マイナーバージョンが指定されたもの以上」という意味です。
Gemfileの書き方は次のセクションで説明します。

`github-pages`のバージョンは比較的頻繁に変わるので注意しましょう。
バージョンの変更によってはサイト全体の見直しが必要になる可能性もあります。

Gemfileを記述したら、Bundlerでインストールします。
BundlerはRuby2.6.0から標準添付されているので、特にインストールの必要はありません。

```
$ bundle install
Fetching gem metadata from https://rubygems.org/...........
Resolving dependencies...
Using public_suffix 4.0.7
Using bundler 2.3.16
... ... ...
... ... ...
$
```

「bundle」はBundlerを端末から起動するためのコマンド名です。
「install」はgemをインストールするサブコマンドです。
BundlerはGemfileを読み、そこに記述されているgemとその依存gemのバージョンをすべて調べ、「Gemfile.lock」というファイルに書き出します。
さらに、それらすべてをインストールします。
「Gemfile.lock」に書かれたバージョンのgemを起動するには「bundle exec」コマンドを使います。
例えば「bundle exec jekyll serve」は「jekyll」をserve引数付きで起動します。
「bundle exec」なしで「jekyll serve」としないでください。
もしそうすると「Gemfile.lock」とは無関係にjekyllを起動することになるのでバージョンの違うJekyllが起動される可能性があります。
また、Jekyllが使用するgemも異なるバージョンが呼び出される可能性があります。

したがって、レポジトリ内では常に「bundle exec」をつけてgemを起動するようにしてください。

```
$ bundle exec jekyll serve
Configuration file: /（クローンしたディレクトリ）/jekyll-tutorial-for-beginners/_config.yml
            Source: /（クローンしたディレクトリ）/jekyll-tutorial-for-beginners
       Destination: /（クローンしたディレクトリ）/jekyll-tutorial-for-beginners/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 0.129 seconds.
 Auto-regeneration: enabled for '/（クローンしたディレクトリ）/jekyll-tutorial-for-beginners'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

下から5行目に警告メッセージが出ていますが、ここでは特に問題がないので無視してください。
この章の最後に警告メッセージが出ないようにする方法を説明します。

これでローカルのウェブサービスが立ち上がって、ブラウザでサイトの内容が確認できます。
ブラウザで、`localhost:4000`を開いてみてください。
GitHub Pagesを見たときと同じ画面が現れるはずです。

### Gemfileの利点

BundlerはGemfileに書かれたgemから、それが依存するgemを調べ、Gemfile.lockに書き出します。
Gemfileよりもかなり多くのgemが追加されることが多いです。
これをいちいちインストールする手間を考えれば、GemfileとBundlerの便利さが分かります。

しかし、それだけではありません。
最も重要なことはバージョン管理をサポートしていることです。
Gemfileでは、gemの後に、そのバージョンを指定することができます。

```ruby
gem 'abcd'
gem 'efgh', '3.0.0'
gem 'ijkl',  '>=1.0'
gem 'mnop',  '~>2.1'
```

- `abcd`は、最新のものがインストールされる（バージョン指定無しの場合）
- `efgh`はバージョン`3.0.0`のものがインストールされる
- `ijkl`のバージョン`1.0`以上のものがインストールされる
- `mnop`はバージョンが`2.1`以上で`3.0`未満がインストールされる。
`~>`では、最後のドットの前は変わらない。
例えば`~>1.2.3.4`は`1.2.3.4`以上`1.2.4`未満。

一般に、公開されたRubyプログラムがGemfileを持っていれば、それをダウンロードした誰もが同じバージョンを使うことになります。
それによって、バージョンの違いによる誤動作を防ぐことができます。

### jekyll-github-metadataの警告メッセージ

この章の最後に警告メッセージについて説明します。

GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.

はじめに「GitHub Metadata:」とあるので、`jekyll-github-metadata` gemの発出したメッセージであることがわかります。
このgemは`gethub-pages`でインストールが指定されていたgemの1つです。
メッセージは、「GitHub APIの認証が見つかりませんでした。 一部のフィールドが欠落しているか、データが正しくない可能性があります。」ということですが、これだけでは何のことかわかりません。
[jekyll-github-metadataのウェブサイトのAuthenticationページ](http://jekyll.github.io/github-metadata/authentication/)を見ると、次のようなことが書いてあります。

- cname などの一部のフィールドでは、自分自身を認証する必要がある。
- それを解決するにはいくつかの方法がある
- JEKYLL_GITHUB_TOKEN環境変数にPAT（第7章で説明するhttps接続のために生成するパーソナル・アクセス・トークンのこと）をセットしてJekyllを起動する
- `netrc` gemを用い、`~/.netrc`にユーザ名とPATを書く
- OCTOKIT_ACCESS_TOKEN環境変数にPATをセットしてJekyllを起動する（1番目とほぼ同じ）

この3つのうちのどれかをすれば警告メッセージは無くなります。
（ただ、警告はエラーではないので、無視してもそれほど大きな問題にはなりません）。
1番目と3番目の環境変数を使うのが簡単ですが、毎回これをするのは煩わしいです。
その点、2番めは一度セットすれば後は何もしなくて良いのが利点です。

まず、`~/.netrc`があるかどうかチェックします。
（`~`はユーザの（あなたの）ホームディレクトリです。ドットから始まるファイルは隠しファイルなので、CTRL+Hで表示できるようにして確認します）。
もしある場合は下記の内容を付け足します。
なければ新規にファイルを作り下記の内容で保存します。

```
machine api.github.com
    login ＜GitHubのユーザ名＞
    password ＜ＰＡＴ＞
```

次に、Gemfileに`netrc` gemを加えます。

```ruby
source "https://rubygems.org"

gem "github-pages", "~> 227", group: :jekyll_plugins

gem "webrick"
gem "netrc"
```

Gemfile.lockを削除し、`bundle install`します。
これで`bundle exec jekyll serve`のときに、警告は出なくなります。
