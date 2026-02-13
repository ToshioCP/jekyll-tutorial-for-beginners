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
