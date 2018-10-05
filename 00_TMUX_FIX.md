# tmux 2.5 以降において East Asian Ambiguous Character を全角文字の幅で表示する

## 概要

[tmux 2.5][TMUX] 以降において、 Unicode の規格における東アジア圏の各種文字のうち、いわゆる "◎" や "★" 等の記号文字及び罫線文字等、 [East_Asian_Width 特性の値が A (Ambiguous) となる文字][EAWA] (以下、 [East Asian Ambiguous Character][EAWA]) が、日本語環境で文字幅を適切に扱うことが出来ずに表示が乱れる問題が発生しています。

ファイル ```tmux-x.y-fix.diff (ここに、 x.y は tmux の安定版のバージョン番号。以下同様)``` 及び ```tmux-HEAD-*-fix.diff``` は、 [tmux 2.5][TMUX] 以降において [East Asian Ambiguous Character][EAWA] の幅を漢字や全角カナ文字等と同じ幅 2 で表示するように修正するための差分ファイルです。

この差分には、　[waltarix 氏][WALT]によって作成された [tmux][TMUX] の画面分割において、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分ファイルである ```pane-border-ascii.patch``` が含まれています。

[https://gist.githubusercontent.com/waltarix/1399751/raw/6c8f54ec8e55823fb99b644a8a5603847cb60882/tmux-pane-border-ascii.patch][PANE]

## 差分ファイルの適用とインストール

[tmux][TMUX] のソースコードに差分ファイルを適用するには、安定版の [tmux][TMUX] には、差分ファイル ```tmux-x.y-fix.diff``` を、 [github 上の tmux の HEAD][TMRP] のソースコードには、　```tmux-HEAD-*-fix.diff``` をそれぞれ適用して下さい。

従って、安定版の [tmux][TMUX] のソースコードにおける差分ファイルについては、 [tmux][TMUX] のソースコードが置かれているディレクトリより、以下のようにして差分ファイル ```tmux-x.y-fix.diff``` を適用後、[tmux][TMUX] を通常通りにビルドしてインストールすると、 [tmux][TMUX] において、 [East Asian Ambiguous Character][EAWA] が全角文字の幅と同じ幅で表示されるようになります。

```
 $ patch -p1 < /path/to/diff/tmux-x.y-fix.diff
 (ここに、/path/to/diff は、 tmux-x.y-fix.diff が置かれたディレクトリのパス名)
```

また、[github 上の tmux の HEAD][TMRP] のソースコードにおける差分ファイルについても、 [github 上の tmux の HEAD][TMRP] のソースコードが置かれているディレクトリより、以下のようにして、最近の差分ファイルのみを適用後、 [tmux の HEAD 版][TMRP]を通常通りにビルドしてインストールすると、 [tmux][TMUX] において、 [East Asian Ambiguous Character][EAWA] が全角文字の幅と同じ幅で表示されるようになります。

```
 $ patch -p1 < /path/to/diff/tmux-HEAD-xxxxxxxx-fix.diff
 (ここに、 tmux-HEAD-xxxxxxxx-fix.diff は、最近の差分ファイルのファイル名であり、 /path/to/diff は、 tmux-HEAD-xxxxxxxx-fix.diff が置かれたディレクトリのパス名)
```

## Linuxbrew を用いた差分ファイルの適用とインストール

[Linuxbrew][BREW] を導入した端末において、[本稿の Gist][GST1] にある差分ファイルを適用した [tmux][TMUX] をインストールする際には、**これらの差分ファイルを適用した [tmux][TMUX] を導入するための [Linuxbrew][BREW] 向け Tap リポジトリ [z80oolong/tmux][TAP1] を使用することを強く御勧め致します。**

Tap リポジトリ [z80oolong/tmux][TAP1] では、[最新の安定版の tmux][TMUX] 及び [github 上の最新の tmux の HEAD][TMRP] のインストールの他、旧安定版の [tmux][TMUX] のインストールも可能です。 

[z80oolong/tmux][TAP1] の詳細な使用法につきましては、 [z80oolong/tmux の README.md][READ] を御覧下さい。

## オプションの設定について

ここで、 [East Asian Ambiguous Character][EAWA] の文字幅を 2 ではなく 1 として扱う場合は、[tmux][TMUX] の設定ファイル ```.tmux.conf``` に以下の設定を追記します。

```
set-option -g utf8-cjk off
```

なお、オプション ```utf8-cjk``` の初期値は、 locale に関する環境変数 ```LC_CTYPE``` の値が ```"ja*", "ko*", "zh*"``` の場合は ```on``` となり、それ以外の場合は ```off``` となります。そして、オプション ```utf8-cjk``` の値が ```on``` の場合は、オプション ```pane-border-ascii``` の値に関わらず、 tmux の画面分割のボーダーラインは ascii 文字となります。

## 謝辞

先ず最初に、[tmux][TMUX] の画面分割において、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分ファイルである ```pane-border-ascii.patch``` を作成された [waltarix 氏][WALT]に心より感謝致します。

また、差分ファイル ```tmux-2.3-fix.diff``` を作成するに当たっては、下記の URL にある、 Markus Kuhn 氏が作成した [East_Asian_Width 特性が A の文字][EAWA]の扱いを考慮した wcwidth(3) 関数の実装を使用しました。 [Markus Kuhn][DRMK] 氏には心より感謝いたします。

[http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c][WCWD]

最後に、 [tmux][TMUX] の作者である [Nicholas Marriott 氏][NICM]を初め、 [tmux][TMUX] に関わる全ての人々に心より感謝致します。

## 追記

### 2017/03/10 現在の追記

なお、 2017/03/10 現在、 [github 上の tmux][TMRP] においては、後述する ```pane-border-ascii.patch``` 及び tmux 2.3 に適用する差分ファイル ```tmux-2.3.diff``` が正常に適用されません。

代わりに、 tmux に差分ファイル ```tmux-HEAD-392253f0-fix.diff``` を適用して下さい。これにより、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分と East Asian Ambiguous Character を全角文字の幅と同じ幅で表示するための差分が同時に適用されるようになります。

### 2017/07/06 現在の追記

2017/07/06 現在、 tmux の[安定版のバージョンは 2.5 になっています][TM25]。以降は tmux 2.5 には差分ファイル ```tmux-2.5-fix.diff``` を、 [github 上の tmux][TMRP] には、 ```tmux-HEAD-6b1ceca8-fix.diff``` をそれぞれ適用して下さい。

### 2017/08/06 現在の追記

2017/08/06 現在の [github 上の tmux の HEAD の commit は e7b1e05b になっています][TMRP]ので、以降は差分ファイル ```tmux-HEAD-e7b1e05b-fix.diff``` を適用して下さい。

### 2017/08/24 現在の追記

2017/08/24 現在の [github 上の tmux の HEAD の commit は 3b40f8e4 になっています][TMRP]ので、以降は差分ファイル ```tmux-HEAD-3b40f8e4-fix.diff``` を適用して下さい。

なお、[github 上の tmux の HEAD][TMRP]に適用する差分ファイルのうち、既に obsolute となった ```tmux-HEAD-392253f0-fix.diff```, ```tmux-HEAD-6b1ceca8-fix.diff```, ```tmux-HEAD-e7b1e05b-fix.diff``` を削除しましたので、どうか御了承下さい。

### 2017/09/17 現在の追記

2017/09/17 現在の [github 上の tmux の HEAD の commit である ae5a62a5][TMRP]に対応した差分ファイル ```tmux-HEAD-ae5a62a5-fix.diff``` を追加致しました。現在の [github 上の tmux の HEAD][TMRP]には ```tmux-HEAD-ae5a62a5-fix.diff``` を適用して下さい。

今回の差分ファイルでは、ソースファイル ```utf8.c``` のコンパイル時に発生する警告に対応するための修正を行っています。

なお、[github 上の tmux の HEAD][TMRP]に適用する差分ファイルのうち、 ```tmux-HEAD-3b40f8e4-fix.diff``` を削除しましたので、どうか御了承下さい。

### 2017/10/17 現在の追記

2017/10/17 現在の tmux の[安定版のバージョンの 2.6][TM26] 及び [github 上の tmux の HEAD の commit である fb02df66][TMRP]に対応した差分ファイル ```tmux-HEAD-fb02df66-fix.diff``` を追加致しました。

なお、[github 上の tmux の HEAD][TMRP]に適用する差分ファイルのうち、 obsolute となった ```tmux-HEAD-ae5a62a5-fix.diff``` を削除しましたので、どうか御了承下さい。

### 2018/01/25 現在の追記とお断り

2018/01/25 現在の [github 上の tmux の HEAD の commit である 19afd842][TMRP]に対応した差分ファイル ```tmux-HEAD-19afd842-fix.diff``` を追加致しました。ここ最近は各種諸用が重なり、本差分ファイルの更新が滞りましたことを心より御詫び申し上げます。

また、 [github 上の tmux の HEAD][TMRP]に適用する差分ファイルのうち、 obsolute となった ```tmux-HEAD-fb02df66-fix.diff``` を削除しましたので、どうか御了承下さい。

また、本差分ファイルの更新に当たって御指摘及び御助力等を頂きました [mattn 氏][MATN]に心より感謝致します。

### 2018/02/23 現在の追記

2018/02/23 現在の [github 上の tmux の HEAD の commit である 9464b94f][TMRP] に対応した差分ファイル ```tmux-HEAD-9464b94f-fix.diff``` を追加致しました。

### 2018/03/23 現在の追記

2018/03/23 現在の [github 上の tmux の HEAD の commit である 919f55ac][TMRP] に対応した差分ファイル ```tmux-HEAD-919f55ac-fix.diff``` を追加致しました。 [github 上の tmux の HEAD の commit である 919f55ac][TMRP] は、安定版 2.7 の rc 版です。

これに伴い、差分ファイル ```tmux-HEAD-19afd842-fix.diff```, ```tmux-HEAD-9464b94f-fix.diff``` を削除しました。どうか御了承下さい。

### 2018/04/06 現在の追記

2018/04/06 現在の [github 上の tmux の HEAD の commit である b5c0b2ca][TMRP] に対応した差分ファイル ```tmux-HEAD-b5c0b2ca-fix.diff``` を追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-919f55ac-fix.diff``` を削除しました。どうか御了承下さい。

なお、今後の差分ファイルについては、[本稿の Gist][GST1] にも投稿を行う他、 [Github のリポジトリ https://github.com/z80oolong/diffs][GITH] にも同時に投稿を行う予定ですので、どうか御了承下さい。

### 2018/04/16 現在の追記

2018/04/16 現在の tmux の[安定版のバージョンの 2.7][TM27] 及び [github 上の tmux の HEAD の commit である ae0b7c7d][TMRP]に対応した差分ファイルである ```tmux-2.7-fix.diff, tmux-HEAD-ae0b7c7d-fix.diff``` を追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-b5c9b2ca-fix.diff``` を削除しました。どうか御了承下さい。

なお、これに伴い、 East Asian Ambiguous Character を全角文字の幅で表示する [tmux][TMUX] を導入するための Formula 群である [z80oolong/tmux][TAP1] を更新しました。こちらの方もどうか御覧下さい。

### 2018/05/15 現在の追記

2018/05/15 現在の [github 上の tmux の HEAD の commit である 82c0eed3][TMRP]に対応した差分ファイルである ```tmux-HEAD-82c0eed3-fix.diff``` を追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-ae0b7c7d-fix.diff``` を削除しました。どうか御了承下さい。

なお、これに伴い、 East Asian Ambiguous Character を全角文字の幅で表示する [tmux][TMUX] を導入するための Formula 群である [z80oolong/tmux][TAP1] を更新しました。こちらの方もどうか御覧下さい。

### 2018/06/12 現在の追記

2018/06/12 現在の [github 上の tmux の HEAD の commit である 9da78d72][TMRP] に対応した差分ファイルである ```tmux-HEAD-9da78d72-fix.diff``` を追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-82c0eed3-fix.diff``` を削除しました。どうか御了承下さい。

なお、これに伴い、 East Asian Ambiguous Character を全角文字の幅で表示する [tmux][TMUX] を導入するための Formula 群である [z80oolong/tmux][TAP1] を更新しました。こちらの方もどうか御覧下さい。

### 2018/08/04 現在の追記

2018/08/04 現在の [github 上の tmux の HEAD の commit である 33f9b316][TMRP] に対応した差分ファイルである ```tmux-HEAD-33f9b316-fix.diff``` を追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-9da78d72-fix.diff``` を削除しました。また、旧安定版に対応した差分ファイルである ```tmux-2.5-fix.diff, tmux-2.6-fix.diff``` も同時に削除しました。どうか御了承下さい。

**なお、旧安定版の [tmux][TMUX] の差分ファイルの適用とインストールは、後述する [Linuxbrew][BREW] 向けの Tap リポジトリ [z80oolong/tmux][TAP1] を用いて行うことが可能です。**

なお、これに伴い、 East Asian Ambiguous Character を全角文字の幅で表示する [tmux][TMUX] を導入するための Formula 群である [z80oolong/tmux][TAP1] を更新しました。こちらの方もどうか御覧下さい。

### 2018/09/12 現在の追記

2018/09/12 現在の [github 上の tmux の HEAD の commit である 71d2ab18][TMRP] に対応した差分ファイルである ```tmux-HEAD-71d2ab18-fix.diff``` を追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-33f9b316-fix.diff``` を削除しました。

また、旧安定版 [tmux 2.3][TMUX] に対応した差分ファイルである ```tmux-2.3-fix.diff``` を削除しました。**今後は、旧安定版の [tmux][TMUX] の差分ファイルの適用とインストールは、 [Linuxbrew][BREW] 向けの Tap リポジトリ [z80oolong/tmux][TAP1] を用いることを推奨します。**

なお、旧安定版 [tmux 2.3][TMUX] に対応した差分ファイルである ```tmux-2.3-fix.diff``` は以下の URL からも入手できます。

[https://raw.githubusercontent.com/z80oolong/diffs/master/tmux/tmux-2.3-fix.diff][DIF1]

旧安定版 [tmux 2.3][TMUX] に対応した差分ファイルである ```tmux-2.3-fix.diff``` を旧安定版 [tmux 2.3][TMUX] のソースコードに適用するには、予め [waltarix 氏][WALT]によって作成された [tmux][TMUX] の画面分割において、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分ファイルである以下の差分ファイルを適用する必要があります。

[https://gist.githubusercontent.com/waltarix/1399751/raw/6c8f54ec8e55823fb99b644a8a5603847cb60882/tmux-pane-border-ascii.patch][PANE]

これに伴い、 East Asian Ambiguous Character を全角文字の幅で表示する [tmux][TMUX] を導入するための Formula 群である [z80oolong/tmux][TAP1] を更新しました。こちらの方もどうか御覧下さい。

### 2018/09/26 現在の追記

2018/09/12 現在の [github 上の tmux の HEAD の commit である 6abb62df][TMRP] に対応した差分ファイルである ```tmux-HEAD-6abb62df-fix.diff``` を追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-71d2ab18-fix.diff``` を削除しました。

また、 East Asian Ambiguous Character を全角文字の幅で表示する [tmux][TMUX] を導入するための Formula 群である [z80oolong/tmux][TAP1] を更新しました。こちらの方もどうか御覧下さい。

### 2018/10/05 現在の追記

2018/10/05 現在の [github 上の tmux の HEAD の commit である 5a7cf897][TMRP] に対応した差分ファイルである ```tmux-HEAD-5a7cf897-fix.diff``` を追加致しました。

また、 [tmux][TMUX] の安定版のプレリリース版である [tmux 2.8-rc][T28R] に対応した差分ファイルである ```tmux-2.8-rc-fix.diff``` も同時に追加致しました。

これに伴い、差分ファイル ```tmux-HEAD-6abb62df-fix.diff``` を削除しました。どうか御了承下さい。

<!-- 外部リンク一覧 -->

[TMUX]:http://tmux.github.io/
[EAWA]:http://www.unicode.org/reports/tr11/#Ambiguous
[TMRP]:https://github.com/tmux/tmux.git
[TM25]:https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz
[WALT]:https://github.com/waltarix
[PANE]:https://gist.githubusercontent.com/waltarix/1399751/raw/6c8f54ec8e55823fb99b644a8a5603847cb60882/tmux-pane-border-ascii.patch
[BREW]:https://linuxbrew.sh
[TAP1]:https://github.com/z80oolong/homebrew-tmux
[READ]:https://github.com/z80oolong/homebrew-tmux/blob/master/README.md
[WCWD]:http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c
[DRMK]:http://www.cl.cam.ac.uk/~mgk25/
[NICM]:https://github.com/nicm
[TM26]:https://github.com/tmux/tmux/releases/download/2.6/tmux-2.6.tar.gz
[TM27]:https://github.com/tmux/tmux/releases/download/2.7/tmux-2.7.tar.gz
[T28R]:https://github.com/tmux/tmux/releases/download/2.8/tmux-2.8-rc.tar.gz
[MATN]:https://github.com/mattn
[GST1]:https://gist.github.com/z80oolong/e65baf0d590f62fab8f4f7c358cbcc34
[GITH]:https://github.com/z80oolong/diffs
[GIT1]:https://github.com/z80oolong/diffs/tree/master/tmux
[DIF1]:https://raw.githubusercontent.com/z80oolong/diffs/master/tmux/tmux-2.3-fix.diff
