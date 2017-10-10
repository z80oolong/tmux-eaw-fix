# tmux 2.3 以降において East Asian Ambiguous Character を全角文字の幅で表示する

## 概要

[tmux 2.3][TMUX] 以降において、 Unicode の規格における東アジア圏の各種文字のうち、いわゆる "◎" や "★" 等の記号文字及び罫線文字等、 [East_Asian_Width 特性の値が A (Ambiguous) となる文字][EAWA] (以下、 [East Asian Ambiguous Character][EAWA]) が、日本語環境で文字幅を適切に扱うことが出来ずに表示が乱れる問題が発生しています。

ファイル ```tmux-2.3-fix.diff```, ```tmux-2.5-fix.diff``` 及び ```tmux-HEAD-*-fix.diff``` は、 [tmux 2.3][TMUX] 以降において [East Asian Ambiguous Character][EAWA] の幅を漢字や全角カナ文字等と同じ幅 2 で表示するように修正するための差分ファイルです。

## 差分ファイルの適用と tmux 2.3 のインストール

[tmux 2.3][TMUX] のソースコードに差分ファイル ```tmux-2.3-fix.diff``` を適用するには、予め、 [waltarix 氏][WALT]によって作成された [tmux][TMUX] の画面分割において、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分ファイルである ```pane-border-ascii.patch``` を適用する必要があります。 

[https://gist.githubusercontent.com/waltarix/1399751/raw/6c8f54ec8e55823fb99b644a8a5603847cb60882/tmux-pane-border-ascii.patch][PANE]

最初に、上記の差分ファイル ```tmux-pane-border-ascii.patch``` を取得した後、[tmux 2.3][TMUX] のソースコードが置かれているディレクトリにおいて、
以下のようにして差分ファイル```tmux-pane-border-ascii.patch``` を適用します。

```
 $ patch -p1 < /path/to/diff/tmux-pane-border-ascii.patch
 (ここに、/path/to/diff は tmux-pane-border-ascii.patch が置かれたディレクトリのパス名)
```

その後、 [tmux 2.3][TMUX] のソースコードが置かれているディレクトリより、以下のようにして差分ファイル ```tmux-2.3-fix.diff``` を適用します。

```
 $ patch -p1 < /path/to/diff/tmux-2.3-fix.diff
 (ここに、/path/to/diff は、 tmux-2.3-fix.diff が置かれたディレクトリのパス名)
```

差分ファイルを適用後、 [tmux 2.3][TMUX] を通常通りにビルドしてインストールすると、 [tmux][TMUX] において、 [East Asian Ambiguous Character][EAWA] が全角文字の幅と同じ幅で表示されるようになります。

## tmux 2.5 及び github 上の tmux の HEAD における差分ファイルの適用とインストール

[tmux 2.5][TMUX] のソースコードに適用する差分ファイル ```tmux-2.5-fix.diff``` には、既に [waltarix 氏][WALT]によって作成された ```pane-border-ascii.patch``` が含まれています。

従って、 [tmux 2.5][TMUX] のソースコードが置かれているディレクトリより、以下のようにして差分ファイル ```tmux-2.5-fix.diff``` のみを適用後、tmux 2.5 を通常通りにビルドしてインストールすると、 [tmux][TMUX] において、 [East Asian Ambiguous Character][EAWA] が全角文字の幅と同じ幅で表示されるようになります。

```
 $ patch -p1 < /path/to/diff/tmux-2.5-fix.diff
 (ここに、/path/to/diff は、 tmux-2.5-fix.diff が置かれたディレクトリのパス名)
```

また、[github 上の tmux の HEAD][TMRP] のソースコードにおける差分ファイルについても、 [github 上の tmux の HEAD][TMRP] のソースコードが置かれているディレクトリより、以下のようにして、最近の差分ファイルのみを適用後、 [tmux の HEAD 版][TMRP]を通常通りにビルドしてインストールすると、 [tmux][TMUX] において、 [East Asian Ambiguous Character][EAWA] が全角文字の幅と同じ幅で表示されるようになります。

```
 $ patch -p1 < /path/to/diff/tmux-HEAD-xxxxxxxx-fix.diff
 (ここに、 tmux-HEAD-xxxxxxxx-fix.diff は、最近の差分ファイルのファイル名であり、 /path/to/diff は、 tmux-HEAD-xxxxxxxx-fix.diff が置かれたディレクトリのパス名)
```

## オプションの設定について

ここで、 [East Asian Ambiguous Character][EAWA] の文字幅を 2 ではなく 1 として扱う場合は、[tmux][TMUX] の設定ファイル ```.tmux.conf``` に以下の設定を追記します。

```
set-option -g utf8-cjk off
```

なお、オプション ```utf8-cjk``` の初期値は、 locale に関する環境変数 ```LC_CTYPE``` の値が ```"ja*", "ko*", "zh*"``` の場合は ```on``` となり、それ以外の場合は ```off``` となります。そして、オプション ```utf8-cjk``` の値が ```on``` の場合は、オプション ```pane-border-ascii``` の値に関わらず、 tmux の画面分割のボーダーラインは ascii 文字となります。

## 最後に

先ず最初に、[tmux][TMUX] の画面分割において、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分ファイルである ```pane-border-ascii.patch``` を作成された [waltarix 氏][WALT]に心より感謝致します。

また、差分ファイル ```tmux-2.3-fix.diff``` を作成するに当たっては、下記の URL にある、 Markus Kuhn 氏が作成した [East_Asian_Width 特性が A の文字][WAEA]の扱いを考慮した wcwidth(3) 関数の実装を使用しました。 [Markus Kuhn][DRMK] 氏には心より感謝いたします。

[http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c][WCWD]

最後に、 [tmux][TMUX] の作者である [Nicholas Marriott 氏][NICM]を初め、 [tmux][TMUX] に関わる全ての人々に心より感謝致します。

## 追記

### 2017/03/10 現在の追記

なお、 2017/03/10 現在、 [github 上の tmux][TMRP] においては、後述する ```pane-border-ascii.patch``` 及び tmux 2.3 に適用する差分ファイル ```tmux-2.3.diff``` が正常に適用されません。

代わりに、 tmux に差分ファイル ```tmux-HEAD-392253f0-fix.diff``` を適用して下さい。これにより、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分と East Asian Ambiguous Character を全角文字の幅と同じ幅で表示するための差分が同時に適用されるようになります。

### 2017/07/06 現在の追記

2017/07/06 現在、 tmux の[安定版のバージョンは 2.5 になっています][TM25]。以降は tmux 2.5 には差分ファイル ```tmux-2.5-fix.diff``` を、 [github 上の tmux][2] には、 ```tmux-HEAD-6b1ceca8-fix.diff``` をそれぞれ適用して下さい。

### 2017/08/06 現在の追記

2017/08/06 現在の [github 上の tmux の HEAD の commit は e7b1e05b になっています][TMRP]ので、以降は差分ファイル ```tmux-HEAD-e7b1e05b-fix.diff``` を適用して下さい。

### 2017/08/24 現在の追記

2017/08/24 現在の [github 上の tmux の HEAD の commit は 3b40f8e4 になっています][TMRP]ので、以降は差分ファイル ```tmux-HEAD-3b40f8e4-fix.diff``` を適用して下さい。

なお、[github 上の tmux の HEAD][TMRP]に適用する差分ファイルのうち、既に obsolute となった ```tmux-HEAD-392253f0-fix.diff```, ```tmux-HEAD-6b1ceca8-fix.diff```, ```tmux-HEAD-e7b1e05b-fix.diff``` を削除しましたので、どうか御了承下さい。

### 2017/09/17 現在の追記

2017/09/17 現在の [github 上の tmux の HEAD の commit である ae5a62a5][TMRP]に対応した差分ファイル ```tmux-HEAD-ae5a62a5-fix.diff``` を追加致しました。現在の [github 上の tmux の HEAD][TMRP]には ```tmux-HEAD-ae5a62a5-fix.diff``` を適用して下さい。

今回の差分ファイルでは、ソースファイル ```utf8.c``` のコンパイル時に発生する警告に対応するための修正を行っています。

なお、[github 上の tmux の HEAD][TMRP]に適用する差分ファイルのうち、 ```tmux-HEAD-3b40f8e4-fix.diff``` を削除しましたので、どうか御了承下さい。

<!-- 外部リンク一覧 -->

[TMUX]:http://tmux.github.io/
[EAWA]:http://www.unicode.org/reports/tr11/#Ambiguous
[TMRP]:https://github.com/tmux/tmux.git
[TM25]:https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz
[WALT]:https://github.com/waltarix
[PANE]:https://gist.githubusercontent.com/waltarix/1399751/raw/6c8f54ec8e55823fb99b644a8a5603847cb60882/tmux-pane-border-ascii.patch
[WCWD]:http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c
[DRMK]:http://www.cl.cam.ac.uk/~mgk25/
[NICM]:https://github.com/nicm