# tmux-eaw-fix -- tmux 2.6 以降において各種問題を修正する野良差分ファイル

## 概要

[tmux][TMUX] とは、 terminal session を効果的に操作できる端末多重化ソフトウェアです。 [tmux][TMUX] の使用により、複数の仮想 window や pane を同時に表示し、それらの間を切り替えたり terminal session を分割することが可能となります。そして、これにより単一の terminal window 内で複数の task を同時に実行することが可能となります。

しかし [tmux 2.6][TMUX] 以降において、現在のところ以下のような問題が発生しています。

- Unicode の規格における東アジア圏の各種文字のうち、いわゆる "◎" や "★" 等の記号文字及び罫線文字等、 [East_Asian_Width 特性の値が A (Ambiguous) となる文字][EAWA] (以下、 [East Asian Ambiguous Character][EAWA]) が、日本語環境で文字幅を適切に扱うことが出来ずに表示が乱れる問題が発生する。
- Unicode 上の絵文字の文字幅も適切に扱われない問題が発生する。
- [tmux][TMUX] の pane 分割において、画面分割におけるボーダーラインの罫線文字の文字幅が適切に扱われず、画面表示が乱れる問題が発生する。
- [tmux][TMUX] の新たな HEAD 版より追加された SIXEL による画像表示において、パレット数が 0 となるような画像と表示させようとすると、 [tmux][TMUX] の process が異常終了したり、 ORMODE に対応した SIXEL 画像が正常に表示されない問題が発生する。

本リポジトリに置かれているファイル ```tmux-x.y-fix.diff (ここに、 x.y は tmux の安定版のバージョン番号。以下同様)``` 及び ```tmux-HEAD-xxxxxxxxx-fix.diff (ここに、 xxxxxxxx は tmux の HEAD 版の最新の commit ID 番号。以下同様)``` は、 [tmux 2.5][TMUX] 以降において、上記で示した問題を解決するために以下で述べる修正を加えた差分ファイルです。

- [East Asian Ambiguous Character][EAWA] の幅を漢字や全角カナ文字等と同じ幅 2 で表示するための各種設定を追加する修正。
- Unicode 上の絵文字の幅を漢字や全角カナ文字等と同じ幅 2 で表示するための各種設定を追加する修正。
- [koie-hidetaka 氏][KOIE]によって作成された [tmux][TMUX] の[画面分割におけるボーダーラインの罫線文字を判別し、適切に描画するためのソースコードの修正][PANE]。
- SIXEL による画像表示において、パレット数が 0 色である画像を表示させると [tmux][TMUX] のプロセス全体が異常終了する問題を修正。
- SIXEL による画像表示において、 ORMODE による SXIEL の画像表示に対応させる修正。
- その他、各種雑多な問題を修正。

なお、本差分ファイルは、 "tmux 2.5 以降において East Asian Ambiguous Character を全角文字の幅で表示する" より、本差分ファイルによって修正される箇所が広範に渡る事に伴い、 "各種問題を修正する野良差分ファイル" と本差分ファイルの呼称を改めるものです。

## 差分ファイルの適用とインストール

[tmux][TMUX] のソースコードに差分ファイルを適用するには、安定版の [tmux][TMUX] には、差分ファイル ```tmux-x.y-fix.diff``` を、 [github 上の tmux の HEAD][TMRP] のソースコードには、　```tmux-HEAD-xxxxxxxx-fix.diff``` をそれぞれ適用して下さい。

従って、安定版の [tmux][TMUX] のソースコードにおける差分ファイルについては、 [tmux][TMUX] のソースコードが置かれているディレクトリより、以下のようにして差分ファイル ```tmux-x.y-fix.diff``` を適用後、[tmux][TMUX] をコマンド ```./configure, make``` を用いてビルド及びインストールすると、上述の修正がなされた [tmux][TMUX] が導入されます。

```
 $ patch -p1 < /path/to/diff/tmux-x.y-fix.diff
 (ここに、/path/to/diff は、 tmux-x.y-fix.diff が置かれたディレクトリのパス名)
 $ ./configure --prefix=/path/to/install ...
 (ここに、 /path/to/install は tmux のインストール先。なお、 ./configure の引数は適宜追加すること。)
 $ make
 $ make install
```

また、[github 上の tmux の HEAD][TMRP] のソースコードにおける差分ファイルについても、 [github 上の tmux の HEAD][TMRP] のソースコードが置かれているディレクトリより、以下のようにして、最近の差分ファイルを適用後、 [tmux の HEAD 版][TMRP]をコマンド ```./configure, make``` を用いてビルド及びインストールすると、上述の修正がなされた [tmux][TMUX] が導入されます。

なお、 [tmux の HEAD 版][TMRP]でのビルドの場合、**コマンド ```./configure``` の実行に先立ち、シェルスクリプト ```./autogen.sh``` を実行して ```./configure``` を生成する必要があることに留意する必要があります。**

```
 $ patch -p1 < /path/to/diff/tmux-HEAD-xxxxxxxx-fix.diff
 (ここに、 /path/to/diff は、 tmux-HEAD-xxxxxxxx-fix.diff が置かれたディレクトリのパス名)
 $ sh ./autogen.sh
 $ ./configure --prefix=/path/to/install ...
 (ここに、 /path/to/install は tmux のインストール先。なお、 ./configure の引数は適宜追加すること。)
 $ make
 $ make install
```

## Homebrew for Linux を用いた差分ファイルの適用とインストール

[Homebrew for Linux][BREW] を導入した端末において、本差分ファイルを適用した [tmux][TMUX] をインストールする際には、**これらの差分ファイルを適用した [tmux][TMUX] を導入するための [Homebrew for Linux][BREW] 向け Tap リポジトリ [z80oolong/tmux][TAP1] を使用することを強く御勧め致します。**

Tap リポジトリ [z80oolong/tmux][TAP1] では、[最新の安定版の tmux][TMUX] 及び [github 上の最新の tmux の HEAD][TMRP] のインストールの他、旧安定版の [tmux][TMUX] のインストールも可能です。 

[z80oolong/tmux][TAP1] の詳細な使用法につきましては、 [z80oolong/tmux の README.md][READ] を御覧下さい。

## AppImage パッケージを用いたインストール

[tmux][TMUX] のソースコードへの本差分ファイルの適用及びソースコードのビルドによるインストールが困難な環境及び状況の方に向けて、**本差分ファイルを適用したソースコードからのビルド済みの [tmux][TMUX] の AppImage パッケージを用意しました。ソースコードからのビルド作業がお手数な方は、 [tmux][TMUX] の AppImage パッケージの使用を強く御勧めします。**

[tmux][TMUX] の AppImage パッケージは、以下の URL にて配布されています。詳細な使用法についても、以下の URL を御覧下さい。

- 野良差分ファイル適用 tmux を起動する AppImage ファイルの配布ページ
    - [https://github.com/z80oolong/tmux-eaw-appimage/releases][APPR]

## 各種設定について

本節では、本差分ファイルを適用後に拡張される [tmux][TMUX] のオプション及び [tmux][TMUX] が参照する環境変数について述べます。

### ```utf8-cjk```

[East Asian Ambiguous Character][EAWA] の文字幅を 2 とすることを有効化するかどうかを設定するオプションです。

このオプションの設定値を ```on``` とすると、 [East Asian Ambiguous Character][EAWA] の文字幅が 2 となり、 ```off``` とすると、文字幅が 1 となります。例えば、[East Asian Ambiguous Character][EAWA] を全角文字として表示する場合は、 [tmux][TMUX] の設定ファイル ```.tmux.conf``` に以下の設定を追記します。

```
set-option -g utf8-cjk on
```

なお、オプション ```utf8-cjk``` の初期値は、 locale に関する環境変数 ```LC_CTYPE``` の値が ```"ja*", "ko*", "zh*"``` の場合は ```on``` となり、それ以外の場合は ```off``` となります。

### ```utf8-emoji```

UTF-8 で定義される絵文字の文字幅を 2 とすることを有効化するかどうかを設定するオプションです。

このオプションの設定値を ```on``` とすると、 UTF-8 で定義される絵文字の文字幅が 2 となり、 ```off``` とすると、文字幅が 1 となります。例えば、 UTF-8 で定義される絵文字のを全角文字として表示する場合は、 [tmux][TMUX] の設定ファイル ```.tmux.conf``` に以下の設定を追記します。

```
set-option -g utf8-emoji on
```

なお、オプション ```utf8-emoji``` の初期値は、 locale に関する環境変数 ```LC_CTYPE``` の値が ```"ja*", "ko*", "zh*"``` の場合は ```on``` となり、それ以外の場合は ```off``` となります。

### ```pane-border-ascii```

[tmux][TMUX] において画面分割を行う場合に罫線文字に ascii 文字を使用するためのオプションです。

通常は [tmux][TMUX] での画面分割において使用する罫線文字は、環境に応じて UTF-8 の罫線文字か、端末が対応している ACS か、若しくは ascii 文字が使用されます。

このオプションを ```on``` に指定すると、 [tmux][TMUX] での画面分割において使用する罫線文字に ascii 文字を使用します。

### ```pane-border-acs```

[tmux][TMUX] において画面分割を行う場合に罫線の描画に ACS を使用するためのオプションです。

このオプションを ```on``` に指定すると、 [tmux][TMUX] での画面分割において罫線の描画に ACS を使用します。

なお、このオプションと ```pane-border-ascii``` の両方を ```on``` に指定した場合は、 ```pane-border-acs``` が有効となることに留意する必要があります。

### 環境変数 ```TMUX_ACS```

環境変数 ```TMUX_ACS``` に以下の値を設定すると、[tmux][TMUX] での画面分割において以下のように罫線の描画を行います。

- ```utf8, utf-8``` … 罫線の文字に UTF-8 の罫線文字を使用します。
- ```acs``` … 罫線の描画に ACS を使用します。
- ```ascii``` … 罫線の文字に ascii 文字を使用します。

なお、環境変数 ```TMUX_ACS``` による設定は、オプション ```pane-border-ascii, pane-border-acs``` の設定に優先する事に留意する必要があります。

### 環境変数 ```TMUX_CONF```

[tmux 2.6-3.0a][TMUX] までにおいては、環境変数 ```TMUX_CONF``` の値には、 System-wide で使用する [tmux][TMUX] の設定ファイル ```tmux.conf``` の絶対パスを指定します。

また、 [tmux 3.1][TMUX] 以降においては、環境変数 ```TMUX_CONF``` の値には、 [tmux][TMUX] において使用する設定ファイル ```tmux.conf``` の絶対パスを、設定ファイルを読み込む順にセミコロンで区切って指定します。なお、 ```tmux.conf``` のパスには、 ```~/``` 及び、先頭に ```$``` を付した環境変数を使用できます。

環境変数 ```TMUX_CONF``` が指定されない場合は、 [tmux][TMUX] のコンパイル時に指定されるマクロ ```TMUX_CONF``` の値が使用されます。

なお、この環境変数 ```TMUX_CONF``` は、 AppImage パッケージファイルにて主に使用されます。

## 謝辞

先ず最初に、本差分ファイルを作成するに当たっては、下記の URL にある、 Markus Kuhn 氏が作成した [East_Asian_Width 特性が A の文字][EAWA]の扱いを考慮した wcwidth(3) 関数の実装を使用しました。 [Markus Kuhn][DRMK] 氏には心より感謝いたします。

- [http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c][WCWD]

また、本差分ファイルについて、 [tmux][TMUX] の画面分割の為のボーダーラインの罫線文字について判別と適切な描画を行う為の修正を作成して頂いた [koie-hidetaka 氏][KOIE]に心より感謝致します。 [koie-hidetaka 氏][KOIE]におきましては、他にも本差分ファイルに関して有益な指摘も幾つか頂きました。

最後に、 [tmux][TMUX] の作者である [Nicholas Marriott 氏][NICM]を初めとする [tmux の開発コミュニティ][TMUX]及び [tmux][TMUX] に関わる全ての人々に心より感謝致します。

## 使用条件

本差分ファイルは、端末多重化ソフトウェアである [tmux][TMUX] に適用する差分ファイルであり、以下に述べる各氏が著作権を有し、[tmux][TMUX] のライセンスと同様である [ISC License][ISCL] に基づいて配布されるものとします。

- [tmux の開発コミュニティ][TMUX]の各氏
- [koie-hidetaka 氏][KOIE]
- [Markus Kuhn 氏][DRMK]
- [Z.OOL. (mailto:zool@zool.jpn.org)][ZOOL]

本差分ファイルの使用条件の詳細については、本リポジトリに同梱する ```LICENSE``` を参照して下さい。

<!-- 外部リンク一覧 -->

[TMUX]:http://tmux.github.io/
[EAWA]:http://www.unicode.org/reports/tr11/#Ambiguous
[TMRP]:https://github.com/tmux/tmux.git
[KOIE]:https://github.com/koie
[PANE]:https://github.com/koie/tmux/commit/ac6c53ffd6c2987a3a4a5807df7fc6cca5d6ce88
[BREW]:https://linuxbrew.sh
[TAP1]:https://github.com/z80oolong/homebrew-tmux
[READ]:https://github.com/z80oolong/homebrew-tmux/blob/master/README.md
[APPR]:https://github.com/z80oolong/tmux-eaw-appimage/releases
[WCWD]:http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c
[DRMK]:http://www.cl.cam.ac.uk/~mgk25/
[NICM]:https://github.com/nicm
[ZOOL]:http://zool.jpn.org/
[ISCL]:https://www.isc.org/downloads/software-support-policy/isc-license/
