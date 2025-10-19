# tmux-eaw-fix -- tmux 2.6 以降の各種問題を修正する差分ファイル

## 概要

[tmux][TMUX] は、端末セッションを効率的に管理できる端末多重化ソフトウェアです。複数の仮想ウィンドウやペインを同時に表示し、切り替えやセッションの分割を可能にします。これにより、単一の端末ウィンドウ内で複数のタスクを並行して実行できます。

しかし、[tmux 2.6][TMUX] 以降では以下の問題が確認されています：

- **Unicode の [East Asian Width 特性が A (Ambiguous)][EAWA] である "◎" や "★" などの記号文字や罫線文字（以下、[East Asian Ambiguous Character][EAWA]）が、日本語環境で文字幅を適切に扱えず、表示が乱れる。**
- Unicode の絵文字の文字幅が正しく処理されない。
- ペイン分割時のボーダーラインの罫線文字の文字幅が適切に扱われず、画面表示が乱れる。
- **HEAD 版で追加された SIXEL 画像表示において、パレット数が 0 の画像を表示しようとするとプロセスが異常終了する、または ORMODE に対応した SIXEL 画像が正常に表示されない。**

本リポジトリの差分ファイル ```tmux-x.y-fix.diff```（```x.y``` は安定版のバージョン番号）および ```tmux-HEAD-xxxxxxxx-fix.diff```（```xxxxxxxx``` は HEAD 版の最新コミット ID）は、[tmux 2.5][TMUX] 以降のこれらの問題を解決するための修正を施したものです。修正内容は以下の通りです：

- **[East Asian Ambiguous Character][EAWA] の文字幅を漢字や全角カナと同じ幅 2 で表示する設定を追加。**
- Unicode 絵文字の文字幅を幅 2 で表示する設定を追加。
- [koie-hidetaka 氏][KOIE] による、ペイン分割時のボーダーライン罫線文字を適切に描画する[修正][PANE]。
- **SIXEL 画像表示でパレット数が 0 の場合にプロセスが異常終了する問題を修正。**
- **SIXEL 画像表示で ORMODE に対応する修正。**
- その他、細かな問題の修正。

**本差分ファイルは、[East Asian Ambiguous Character][EAWA] を全角文字幅で表示する修正を中心に、広範な問題を解決する "野良差分ファイル" と位置付けられます。**

## 差分ファイルの適用とインストール

### 安定版 tmux

安定版 [tmux][TMUX] のソースコードに差分ファイル ```tmux-x.y-fix.diff``` を適用するには、以下の手順を実行してください：

```
  $ patch -p1 < /path/to/diff/tmux-x.y-fix.diff
  $ ./configure --prefix=/path/to/install ...
  $ make
  $ make install
```

**ここに、```/path/to/diff``` は差分ファイルのディレクトリ、```/path/to/install``` はインストール先のパスを示します。```./configure``` の引数は適宜追加してください。**

### HEAD 版 tmux

[GitHub の tmux HEAD][TMRP] のソースコードに ```tmux-HEAD-xxxxxxxx-fix.diff``` を適用する場合、以下の手順を実行してください。**HEAD 版では ```./configure``` を生成するために、事前に ```./autogen.sh``` を実行する必要があります。**

```
  $ patch -p1 < /path/to/diff/tmux-HEAD-xxxxxxxx-fix.diff
  $ sh ./autogen.sh
  $ ./configure --prefix=/path/to/install ...
  $ make
  $ make install
```

## Homebrew for Linux を使用したインストール

[Homebrew for Linux][BREW] を使用する場合、**本差分ファイルを適用した [tmux][TMUX] を [z80oolong/tmux][TAP1] Tap リポジトリからインストールすることを強く推奨します。** このリポジトリでは、最新安定版、HEAD 版、旧安定版のインストールが可能です。詳細は [z80oolong/tmux の README][READ] を参照してください。

## AppImage パッケージを使用したインストール

**ソースコードのビルドが困難な環境向けに、差分ファイルを適用した [tmux][TMUX] の AppImage パッケージを用意しました。ビルドの手間を省きたい場合は、以下の URL から配布されている AppImage パッケージの使用を強く推奨します：**

- [tmux-eaw-appimage 配布ページ][APPR]

## 設定オプション

本差分ファイルを適用すると、以下の [tmux][TMUX] オプションおよび環境変数が追加されます。

### ```utf8-cjk```

**[East Asian Ambiguous Character][EAWA] の文字幅を 2（全角）にする設定。```on``` で幅 2、```off``` で幅 1 となります。**

以下は設定例です。

```
  set-option -g utf8-cjk on
```

**初期値は、環境変数 ```LC_CTYPE``` が ```ja*```, ```ko*```, ```zh*``` の場合 ```on```、それ以外は ```off``` となります。**

### ```utf8-emoji```

UTF-8 絵文字の文字幅を 2（全角）にする設定。```on``` で幅 2、```off``` で幅 1。

以下は設定例です。

```
  set-option -g utf8-emoji on
```

**初期値は ```utf8-cjk``` と同様です。**

### ```pane-border-ascii```

ペイン分割時の罫線文字に ASCII 文字を使用する設定です。```on``` で ASCII 文字を使用します。

### ```pane-border-acs```

ペイン分割時の罫線描画に ACS を使用する設定です。```on``` で ACS を使用します。

**```pane-border-ascii``` と両方 ```on``` の場合、```pane-border-acs``` が優先します。**

### 環境変数 ```TMUX_ACS```

ペイン分割時の罫線描画を設定します：

- ```utf8```, ```utf-8```: UTF-8 罫線文字を使用します。
- ```acs```: ACS を使用します。
- ```ascii```: ASCII 文字を使用します。

**```TMUX_ACS``` は ```pane-border-ascii``` および ```pane-border-acs``` に優先します。**

### 環境変数 ```TMUX_CONF```

- [tmux 2.6-3.0a][TMUX]: システム全体の ```tmux.conf``` の絶対パスを指定します。
- [tmux 3.1][TMUX] 以降: 複数の ```tmux.conf``` のパスをセミコロンで区切って指定します（```~/``` や環境変数 ```$``` も使用可）。
- 未指定の場合、コンパイル時のマクロ ```TMUX_CONF``` の値を使用します。

**```TMUX_CONF``` は主に AppImage パッケージで使用されます。**

## 謝辞

本差分ファイルの作成にあたり、[Markus Kuhn 氏][DRMK] の [wcwidth(3) 実装][WCWD] を使用しました。また、ペイン分割の罫線文字の修正を提供してくれた [koie-hidetaka 氏][KOIE]、[tmux][TMUX] の開発者 [Nicholas Marriott 氏][NICM] をはじめとする [tmux コミュニティ][TMUX] に深く感謝します。

## 使用条件

本差分ファイルは [tmux][TMUX] 用の差分ファイルであり、[ISC License][ISCL] の下で配布されます。著作権は以下の者に帰属します：

- [tmux 開発コミュニティ][TMUX]
- [koie-hidetaka 氏][KOIE]
- [Markus Kuhn 氏][DRMK]
- [Z.OOL.][ZOOL]

詳細はリポジトリ内の ```LICENSE``` を参照してください。

<!-- 外部リンク一覧 -->

[TMUX]: http://tmux.github.io/
[EAWA]: http://www.unicode.org/reports/tr11/#Ambiguous
[TMRP]: https://github.com/tmux/tmux.git
[KOIE]: https://github.com/koie
[PANE]: https://github.com/koie/tmux/commit/ac6c53ffd6c2987a3a4a5807df7fc6cca5d6ce88
[BREW]: https://linuxbrew.sh
[TAP1]: https://github.com/z80oolong/homebrew-tmux
[READ]: https://github.com/z80oolong/homebrew-tmux/blob/master/README.md
[APPR]: https://github.com/z80oolong/tmux-eaw-appimage/releases
[WCWD]: http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c
[DRMK]: http://www.cl.cam.ac.uk/~mgk25/
[NICM]: https://github.com/nicm
[ZOOL]: http://zool.jpn.org/
[ISCL]: https://www.isc.org/downloads/software-support-policy/isc-license/
