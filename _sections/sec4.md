---
layout: section
title: ホームページ（index.md）の作成
description: Minimaを使ってホームページを作成する
chap: 1
sec: 4
---

この章からしばらくは、実際に操作をしながら、Jekyllによるウェブページの作成を学んでいきます。

## index.mdのテンプレート

`jekyll new`コマンドで作られた`index.md`のテンプレートを見てみましょう。

```
---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
```

一般に、Jekyllのソースファイルは、3つのハイフンで囲まれた部分とそれ以降の部分に分かれます。
それぞれ、フロントマターとコンテンツと呼びます。

```
---
  フロントマター
---
  コンテンツ
```

上記の`index.md`では、2行目から5行目までがフロントマター、7行目からがコンテンツです。
ただ、このファイルは少々特別で、コンテンツ部分が何もありません。

### フロントマター

フロントマターは、[YAML][1]形式で記述したデータで、ビルドに必要なデータを記述します。
しかし、常にフロントマターが必要なわけではなく、例えば、画像ファイルなどにはフロントマターがありません。
フロントマターのないファイルは「静的ファイル」と呼ばれ、変換されずに`_site`ディレクトリにコピーされます。

`index.md`のフロントマターを見てみましょう。
ハッシュ（`#`）以降の部分はコメントで、ビルドの対象にはなりません。
`layout: home`は、レイアウトについての記述です。
「[homeレイアウト](#homeレイアウト)」の項で説明します。

### コンテンツ

コンテンツは「内容」という意味です。
様々な形式（例えばMarkdownとLiquid）で記述されており、生成ファイルの元になる部分です。
つまり、コンテンツが変換されて生成ファイルになる、ということです。

## homeレイアウト

`index.md`のフロントマターの`layout: home`は、`home.html`というファイルをレイアウトとして用いるという意味です。
レイアウトファイルの記述は少し難しいので、[第3章]({% include get_chap_url.html chap=3 %})で説明します。
しかし、そのレイアウトの適用結果は、生成されたHTMLファイルをブラウザで見れば分かります。
[セクション2]({% include get_sec_url.html sec=2 %})で表示された画面は次のようなものでした。

![My-blog-index]({{ "/assets/images/my-blog-index-structure.png" | relative_url }})

画面は大きく分けて4つの部分からなります。

- ヘッダー
- コンテンツ
- ブログリスト
- フッター

ヘッダーには2つの要素があります。

- タイトル：「Your awesome title」と書かれた部分
- Aboutページへのリンク

フッターは3つのカラム（列）に分かれています。

- 左：ここには表示されていませんが、著者などを表示することができます
- 中央：著者のSNSへのリンク。X、インスタ、Facebookなどへのリンクです
- 右：ブログの説明

## ホームページのカスタマイズ

### タイトルなどのカスタマイズ

タイトルなど、ブログ全体に関する情報は設定ファイル（`_config.yml`）に書きます。

エディタで`_config.yml`を開きましょう。
長いので、以下では一部省略しています。

```
# Welcome to Jekyll!
#

...
...

title: Your awesome title
email: your-email@example.com
description: >- # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jekyllrb
github_username:  jekyll

...
...
```

このファイルの形式はYAMLです。

```
title: Your awesome title
```

これは、（このウェブサイトの）タイトルは「Your awesome title」だ、ということを示したものです。
YAMLでは、コロンの左側をキー（key）、右側を値（value）といいます。
この設定により、ホームページのヘッダーのタイトルが「Your awesome title」と表示されていたのです。
ここをカスタマイズすることによって、タイトルを変更できます。

ここでは、マレーシアのペナン島の紹介ブログを作成例にとってみます。
著者名は「檳城島人」という架空の名前にしました。メールアドレスとGitHubアカウントも架空のものです。
設定ファイル`_config.yml`を書きかえて、次のようにします。

```
# Site settings
title: ペナン島へ行こう
author: 檳城島人
email: shimato-binjo@example.com
description: >- # this means to ignore newlines until "baseurl:"
  ペナン島には世界歴史遺産のジョージタウンをはじめ、魅力ある観光スポットが点在しています。
  このサイトでは、ペナン島の魅力を伝えるための情報を発信します。
baseurl: "" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
github_username:  ShimatoBinjo

# Build settings
markdown: kramdown
theme: minima
plugins:
  - jekyll-feed
```

Jekyllでビルドしてホームページを見てください。

```
$ bundle exec jekyll serve
```

タイトルが変わり、フッターに著者名が入るなど、変更が確認できると思います。

### index.mdのカスタマイズ

フロントマターのコメント（#で始まる部分）は要らないので削除します。

コンテンツには画像を入れることにしましょう。
画像用にディレクトリを作り、そこに`StreetArt.jpg`という画像ファイルを入れます。

```
/assets/images/StreetArt.jpg
```

このパス名の最初のスラッシュは、ソースのルートディレクトリを表します。
Linuxファイルシステムのルートではないので注意してください。

index.mdは次のようになります。

{% raw %}
```
---
layout: home
---

![Street-art]({{ "/assets/images/StreetArt.jpg" | relative_url }})
```
{% endraw %}

Markdownの画像へのリンクの記法では、

```
![代替テキスト](画像ファイルへのリンク)
```

ですが、リンクのところが特別な書き方になっています。
{% raw %}`{{`と`}}`{% endraw %}で囲まれたところは、後に述べるLiquidの記述で、後ほど詳しく述べます。
とりあえず、画像へのリンクはこのように書くのだと思ってください。

ホームページのスクリーンショットです。

![ホームページ（画像）]({{ "/assets/images/my-blog-index-image.png" | relative_url }})

[1]: https://yaml.org/
