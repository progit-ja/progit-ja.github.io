# Pro Git 日本語版電子書籍 公開サイト

[Pro Git 日本語版電子書籍公開サイト](http://progit-ja.github.io/)の README です。

## サイトの更新について

Pro Git リポジトリの[日本語ディレクトリ](https://github.com/progit/progit/tree/master/ja)に変更が加わった場合に、以下の手順で更新を行います。

1. 更新された Markdown ファイルから電子書籍ファイルを生成する
2. 生成されたファイルを GitHub にプッシュする
3. [Releases · progit-ja/progit](https://github.com/progit-ja/progit/releases) に最新版を登録する
4. [更新情報](http://progit-ja.github.io/#news)を更新する

- 参考： [History for ja - progit/progit](https://github.com/progit/progit/commits/master/ja)

## 電子書籍データの生成方法

公開している電子書籍データは Ubuntu で生成しています。手順は以下の通りです(12.04LTS と 13.04 で検証済み)。

    sudo apt-get install -y git pandoc texlive-xetex texlive-latex-extra calibre ruby-rdiscount
    sudo apt-get install -y fonts-ipafont ttf-vlgothic

必須のツール、モジュール、フォント類をインストールします。Tex Live 関連はもう少しスマートなやり方がありそうですが、あれこれ怒られた(`xkeyval.sty` がない、 `framed.sty` がないなど色々怒られました。)ので包括的なパッケージをインストールしています。

    sudo apt-get install -y xvfb

MOBI 形式ファイルの生成時に `SVG rasterizer unavailable, SVG will not be converted` と怒られる場合があるので、xvfb をインストールします。

    git clone https://github.com/progit-ja/progit.git
    cd progit

日本語版公開用に修正したリポジトリからソースを取得していますが、必要に応じて本家 https://github.com/progit/progit.git から最新版のソースを取得してください。

    /bin/bash makepdfs ja
    FORMAT=mobi xvfb-run ruby makeebooks ja
    FORMAT=epub ruby makeebooks ja

特にエラーがでなければ、`progit.ja.pdf` 等のファイル名でファイルが生成されます。

本家の [README](https://github.com/progit/progit#making-ebooks) では Fedora (16) と Mac OS での生成方法が紹介されていますので、そちらも参考にしてください。

### rdiscount のバージョン

電子書籍ファイル生成に使用している `makepdfs` や `makeebooks` コマンドでは、Markdown ファイルの HTML 変換に Ruby の Gem である rdiscount を使用しています。

`gem install rdiscount` でインストールされる rdiscount の最新バージョン (2013-07-24 時点だと 2.1.6)だと Markdown の強調表現( `**` や `__` を `<em>` として扱う)が無視されてしまったので、`apt-get` でインストールできるバージョン (2013-07-24 時点だと 1.6.8)を、上記手順では使用しました。