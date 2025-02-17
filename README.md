
# What’s this?

Customized R Markdown/Bookdown format functions for Japanese users

# なにこれ

-   R Markdown で
    日本語文書を作るためのフォーマットを梱包したパッケージです
-   現時点では以下のようなテンプレートがあります.
    -   Beamer スライド (`Beamer in Japanese`)
    -   PDF 文書 (`pdf article in Japanese`)
    -   縦書きPDF文書 (`pdf vartical writing in Japanese`)
    -   書籍形式 (`pdf book in Japanese`, PDF および gitbook
        テンプレートに沿った HTML
-   XeLaTeXまたはLuaLaTeXでのタイプセットを前提にしています
    -   それぞれ `zxjatype`, `luatex-ja`, を利用して和文表示をしています
-   私的LaTeXテンプレ集である[my\_latex\_templates](https://github.com/Gedevan-Aleksizde/my_latex_templates/)からパッケージとして独立しました
-   MS Office Word の出力がしたい場合は [Wordja](https://github.com/Gedevan-Aleksizde/wordja) を参考にしてください.

以下は rmdja 自身で作成したドキュメントです. HTML/PDF も用意しています.

-   『rmdja による多様な形式の日本語技術文書の作成』:
    <https://gedevan-aleksizde.github.io/rmdja/>
-   Yihui 氏による knitr のドキュメントの日本語訳:
    <https://gedevan-aleksizde.github.io/knitr-doc-ja/index.html>
-   同氏による TinyTeX のドキュメントの日本語訳: <https://gedevan-aleksizde.github.io/tinytex-doc-ja/>
-   “R Markdown Cookbook” の日本語訳
    <https://gedevan-aleksizde.github.io/rmarkdown-cookbook/>

# 最低限必要なもの

-   R (&gt;= 3.6.2)
-   R Studio (&gt;= 1.3.1056)
    -   Windows かつ **reticulate** で Python を使用する場合は [v1.4
        台での不具合](https://ill-identified.hatenablog.com/entry/2021/02/22/233326)に注意してください
-   依存パッケージ
    (通常はインストール時に合わせてインストールされるため,
    手作業でなにかする必要はありません)
    -   **rmarkdown** (&gt;=2.6)
    -   **bookdown** (&gt;=0.21)
    -   **commonmark** (&gt;=1.7)
    -   **styler** (&gt;=1.3.2)
-   PDF を出力したい
    (おそらくこのパッケージに関心を持った方はほとんど当てはまると思います)
    場合は以下の要件も必要です
    -   TeX Live 2020 以降相当の TeX 環境,
        特に原ノ味フォントをインストールしていること[1]

    -   もしくは TeX を完全に**アンインストール**した状態

        -   この場合は **tinytex** パッケージで TeX 環境を再構築します
            (後述).

    -   `cairo_pdf()` が動作する環境 Linux や Mac では必要な X11 や
        cairo がインストールされていない可能性があります.
        以下で確認できます.

        ``` r
        capabilities()[c("cairo", "X11")]
        ```

        もし `FALSE` があるならば, たとえば Mac
        なら以下のようにしてインストールします (homebrew が必要です).

        ``` bash
        # Cairo
        brew install cairo
        # X11
        brew install --cask xquartz
        ```

        **注意**: xquartz に関しては, 公式 <https://www.xquartz.org/>
        からインストールしたほうが良いかもしれません.

# インストールから使用まで

1.  後述の必要なパッケージや外部プログラムをインストールする

2.  このパッケージをインストールする (`remotes`
    パッケージを使うのが簡単です)
    
    ~~`remotes::install_github('Gedevan-Aleksizde/rmdja', upgrade = "never")`~~
    
    **注**: 最近は master が更新されていませんので, development ブランチからのインストールを推奨します. テストが不完全なため master に反映していないだけなので, 基本的には master より development のほうが改良されています. 具体的には以下のようにします.

    `remotes::install_github('Gedevan-Aleksizde/rmdja@development', upgrade = "never")`


    特定のバージョンをインストールする場合は,
    以下のようにして指定できます.
    `remotes::install_github('Gedevan-Aleksizde/rmdja', ref="v0.4", upgrade = "never")`
    または以下のような記法も可能です
    `remotes::install_github('Gedevan-Aleksizde/rmdja@v0.4', upgrade = "never")`

    `@development` は開発中のバージョンです.
    新機能が追加されてる場合もありますがバグも多いです.
    ソースコードを理解して適宜修正できる自信のある方のみ使用してください.

        remotes::install_github('Gedevan-Aleksizde/rmdja@development', upgrade = "never")

    Windows OS では
    [Rtools](https://cran.r-project.org/bin/windows/Rtools/)
    をインストールしていない場合, 上記 `remotes::install_github()`
    では依存パッケージを自動インストールしてくれないことがあります.
    Rtools をインストールするか,
    依存パッケージを手動でインストールしてください. 必要なパッケージは
    [`DESCRIPTION`](DESCRIPTION) の `Imports` の項目に書かれています.

    ``` r
    install.packages(c("rmarkdown", "bookdown", "commonmark", "styler"))
    ```

    リリース一覧からダウンロードしたアーカイブファイルからインストールすることもできます.

3.  TeX 環境を未インストールなら, ここでインストールします
    これはそれなりに時間がかかります. また,
    初回のコンパイルにも追加ダウンロードで時間がかかるかもしれません.

    ``` r
    tinytex::install_tinytex()
    ```

4.  新規作成時に \[R Markdown\] -&gt; \[From Template\] -&gt; `{rmdja}`
    のテンプレートのいずれかを選択します ![template
    selection](inst/resources/img/readme-selection.png)

    -   または最初は
        [`resources/examples/beamer`](inst/resources/examples/)
        にもいくつか使用例があります.

5.  フォントの指定 (オプション)

    -   OSごとの違いはほぼデフォルトのフォントだけです.
        もしフォントが表示されない/気に入らない場合は手動で指定してください.
        例えば,
    -   MS (Win10) なら

    <!-- -->

        jfontpreset: bizud

    -   Ubuntuなら

    <!-- -->

        jfontpreset: noto

    -   Noto が入ってない, または Ubuntu 以外の Linux なら

    <!-- -->

        jfontpreset: haranoaji

    -   macなら

    <!-- -->

        jfontpreset: hiragino-pro

    でとりあえずは動くはずです.

    -   XeLaTeX をお使いなら `zxjafont`, LuaLaTeX をお使いなら
        `luatex-ja` のプリセット名で指定できます
    -   混植も可能です
    -   詳しくは [`examples/beamer`](inst/resources/examples/beamer/)
        以下の pdf を確認してください.

**NOTE**: `jmainfont`, `jsansfont`, `jmonofont`
で書体ごとにフォントを設定できます. `mainfont`/`sansfont`/`monofont`
は欧文用です. 特に
`monofont`/`jamonofont`はソースコードの掲載に使われます.
プログラムの解説をしたい場合は[M+](http://mix-mplus-ipa.osdn.jp/)や[Ricty](https://rictyfonts.github.io/)などのインストールを推奨します

**NOTE**: 現時点での XeLaTeX 版と LuaLaTeX 版の違いは以下のとおりです.

1.  一部のLaTeXコマンドが違う
2.  文字の相対的な大きさ, 字間などのレイアウトが微妙に違う
3.  LuaLaTeX のほうがやや処理が遅い
4.  縦書き文書は LuaLaTeX のみ対応

## 初期バージョン (rmdCJK) をお使いの場合

名称を変更したため旧バージョンは不要になります.
アンインストールしてください

    remove.packages("rmdCJK")

# 用途によっては追加インストールが必要なもの

## (u)pBibTeX を使用したい場合

-   upBibTeX を使って参考文献を出力したい (≒ .bst ファイルを指定したい)
    場合, TeX Live のインストールが必要になります. BibLaTeX や
    pandoc-citeproc で良い,
    参考文献を一切使わないというのであれば**不要**です
-   Mac OS なら MacTeX, Ubuntu
    なら[公式](https://www.tug.org/texlive/acquire-netinstall.html)から落としてください
    -   Ubuntu は `apt` を**使わず**インストールしたほうが良いです
    -   [TeX
        wiki](https://texwiki.texjp.org/?TeX%20Live)などを参考にしてください
-   [`jecon.bst`](https://github.com/ShiroTakeda/jecon-bst)
    が役に立つかも知れません
    -   日本語文献リスト用のスタイルファイルです
    -   TeX Live にも `jplain.bst`, `jipsj.bst`
        などの日本語対応スタイルがバンドルされていますが, `jecon.bst`
        は日本語出力用のオプションが充実しています.

## フォントについて

日本語フォントを指定しなかった場合 (`jfontpreset` 未設定,
かつ`j~~font`の設定が3つ揃っていない場合),
OSを判別して以下のようにデフォルトフォントを決めています. これらは
(Linux 以外) OS標準インストールフォントのはずです.

|          |          Mac | Ubuntu | windows (8以降) | windows(それ以前) | それ以外 |
|:---------|-------------:|-------:|----------------:|------------------:|---------:|
| XeLaTeX  |       游書体 |   Noto |          游書体 |        MSフォント |   原ノ味 |
| LuaLaTeX | ヒラギノProN |   Noto |          游書体 |        MSフォント |   原ノ味 |

Ubuntu 18 以降の設定に準拠して Noto-CJK をデフォルトにしています. 他の
Linux 系 OS ならば, TeX Live 2020 以降で同梱されている原ノ味フォント
(`haranoaji`) がデフォルトになるようにしています.

なお, Noto フォントのインストール方法は以下のようにします

Ubuntu/Debian:

``` sh
sudo apt install fonts-noto-cjk-extra -t stretch-backports
```

Cent OS とか Fedora とか:

<https://www.google.com/get/noto/help/install/>

**NOTE**: `monofont`/`jamonofont`はソースコードの掲載に使われます.
[M+](http://mix-mplus-ipa.osdn.jp/)や[Ricty](https://rictyfonts.github.io/)などのインストールを推奨します

### 注意事項

-   現時点では実際にフォントがインストールされているか判定していません.
-   `XeLaTeX`
    ではヒラギノフォントのプリセット`hiragino-pro`/`hiragino-pron`は, OS
    Xにバンドルされていないヒラギノ明朝 W2を必要とします.
    インストールされていない場合, この設定ではエラーが発生します.
-   WindowsかつLuaLaTeXのとき, `\jfontspec` でフォント変更する歳, Noto
    Serif CJK JP が読み込めないことがあります (原因調査中)
-   LuaLaTeX ではフォントが常にゴシック体になります, また,
    一部のフォントプリセットが正しく認識されないことがあります
    (詳細は公式ドキュメントを参照してください)

## サンプルの内容を再現したい場合

[`examples/`](inst/resources/examples/beamer/)
以下に用例がいくつか存在します.

-   `beamer_xelate.Rmd`
-   `beamer_lualatex.Rmd`

`*.pdf` はそれぞれに対応する出力例です.

各OSでよく使われるフォントを指定している以外は上記は全て同じです.
適当なディレクトリに上記いずれかをコピーしてknitしてみてください.
コピーする際には

    file.copy(system.file("resources/examples/beamer/beamer_xelatex.Rmd", package = "rmdja"), to = "./")
    file.copy(system.file("resources/examples/beamer/beamer_lualatex.Rmd", package = "rmdja"), to = "./")

と言うふうにコピーすると楽です.

**NOTE**: 用例の一環として, knit時に同じフォルダに `tab.tex`,
`examples.bib`, `.latexmkrc` というファイルが生成されます.
上書きに注意してください.

### examples に必要なRパッケージ

-   なくても動きますが, あったほうが使い方がわかりやすいです
-   以下でインストールしてください

<!-- -->

    install.packages(
      c("conflicted", "rmarkdown", "tidyverse", "ggthemes", "ggdag", "DiagrammeR", "xtables", "kableExtra", "stargazer", "tufte"),
      dependencies = T)

-   MacおよびWindowsはさらに以下が必要です (DOT言語での作図例のため)
    -   Windows はさらにRStudioの再起動が必要かもしれません

<!-- -->

    install.packages("webshot")
    webshot::install_install_phantomjs()

### examples に必要な外部プログラム

-   Graphviz

DOT言語で生成した画像を挿入する用例があるため, インストールが必要です

MAC:

    brew install graphviz

Ubuntu:

    sudo apt install graphiviz

-   [BXcoloremoji.sty](https://github.com/zr-tex8r/BXcoloremoji)
    -   カラー絵文字を出力したい場合に必要です.
        CTANに登録されてないため手動インストールする必要があります

# 謝辞

-   以下の数々の資料に触発されて作りました

    -   <https://atusy.github.io/tokyor85-original-rmd-format>
    -   <https://kazutan.github.io/HijiyamaR6/intoTheRmarkdown.html#/>
    -   <https://github.com/atusy/tokyor85down>

-   kazutan 氏による [`bookdown`
    日本語対応テンプレ](https://github.com/kazutan/bookdown_ja_template)

-   および kenjimyzk
    氏によるその[改良版](https://github.com/kenjimyzk/bookdown_ja_template)

-   r-wakalang で `rmarkdown`
    の不具合を指摘するや即座にプルリクエストを上げていただいた Atusy 氏

-   奥村晴彦氏の主催する [TeX Forum](https://oku.edu.mie-u.ac.jp/tex/)
    で質問に答えて頂いた方々

この場を借りて感謝します

# その他

2020/9/19 Tokyo.R で本パッケージの機能を紹介しました.

『[おまえは万物をRSTUDIOで書ける](https://speakerdeck.com/ktgrstsh/you-can-write-everything-on-rstudio)』

# 更新履歴メモ

-   v0.4 以降の更新情報は NEWS.md を参照してください
-   v0.3.2
    -   v0.3の寝起きで作ったおかしいところを修正したバージョン
-   v0.3.1
    -   ミスしたので微修正版 以下はリポジトリ分割前のバージョンです.
-   v0.3
    -   `bookdown` 日本語版に対応
    -   フォントを指定しなかった場合, OSに応じて自動設定するように
    -   複数形式に対応したルビ出力関数 `ruby()` を追加
    -   クリエイティブコモンズのアイコン表示 `get_CC()` を追加
-   v0.2
    -   新規作成時のテンプレートとして選べるように
    -   用例ファイルのフォント選択を自動判別化
        (フロントマターにベタ書きしただけ)
-   v0.1
    -   最初の公開版
-   (0.0.5) LuaLaTeX/XeLaTeX 両方に対応できるように, 再度の名前変更
-   (0.1.0) win/ubuntu/macで対応, XeLaTeX/LuaLaTeX
    で動作確認したのでmasterにマージ.
-   (0.1.1) レイアウト微修正

# その他開発メモ

## TODO: ロードマップ, すこし難しいが追加したい機能

将来やってみたいこと. 詳細なスケジュール未定.

-   biblatex のスタイル整備
-   チャンク出力ブロックの PDF での soft wrap をデフォルトにする
-   同様にコード整形も適切な折返しできないか?
    -   現時点では規格自体が存在しないので難しい
-   mainfont に連動してグラフで使うフォントも変更したい
    -   fontregisterer や ragg との連携も検討
-   バージョン管理: 特に PDF 版にバージョン履歴自動生成したい
    -   html 版は github の対応ページにリンク貼れたりするが pdf
        はどうする?
-   internationalization and localization
    -   bookdown のエンドユーザー向け機能の範囲では無理.
-   はてなブログ他ブログサービスに直接送れるようにする
-   縦書き文書の生成
    -   PDF のみ
-   vivliostyle への対応?
-   epub の出力調整

[1] 2019以前をお使いで,
これから更新する場合は追加の手続きが必要らしいです. 参考:
<https://text.baldanders.info/remark/2020/04/haranoaji-fonts-with-texlive-2020/>
