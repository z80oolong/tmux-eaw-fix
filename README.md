# tmux 2.5 以降において East Asian Ambiguous Character を全角文字の幅で表示する

## 概要

[tmux 2.5][TMUX] 以降において、 Unicode の規格における東アジア圏の各種文字のうち、いわゆる "◎" や "★" 等の記号文字及び罫線文字等、 [East_Asian_Width 特性の値が A (Ambiguous) となる文字][EAWA] (以下、 [East Asian Ambiguous Character][EAWA]) が、日本語環境で文字幅を適切に扱うことが出来ずに表示が乱れる問題が発生しています。

ファイル ```tmux-x.y-fix.diff (ここに、 x.y は tmux の安定版のバージョン番号。以下同様)``` 及び ```tmux-HEAD-xxxxxxxxx-fix.diff (ここに、 xxxxxxxx は tmux の HEAD 版の最新の commit ID 番号。以下同様)``` は、 [tmux 2.5][TMUX] 以降において [East Asian Ambiguous Character][EAWA] の幅を漢字や全角カナ文字等と同じ幅 2 で表示するように修正するための差分ファイルです。

この差分には、　[waltarix 氏][WALT]によって作成された [tmux][TMUX] の画面分割において、ボーダーラインを罫線文字に代えて ascii 文字を使用するための差分ファイルである ```pane-border-ascii.patch``` が含まれています。

[https://gist.githubusercontent.com/waltarix/1399751/raw/6c8f54ec8e55823fb99b644a8a5603847cb60882/tmux-pane-border-ascii.patch][PANE]

## 差分ファイルの適用とインストール

[tmux][TMUX] のソースコードに差分ファイルを適用するには、安定版の [tmux][TMUX] には、差分ファイル ```tmux-x.y-fix.diff``` を、 [github 上の tmux の HEAD][TMRP] のソースコードには、　```tmux-HEAD-xxxxxxxx-fix.diff``` をそれぞれ適用して下さい。

従って、安定版の [tmux][TMUX] のソースコードにおける差分ファイルについては、 [tmux][TMUX] のソースコードが置かれているディレクトリより、以下のようにして差分ファイル ```tmux-x.y-fix.diff``` を適用後、[tmux][TMUX] を通常通りにビルドしてインストールすると、 [tmux][TMUX] において、 [East Asian Ambiguous Character][EAWA] が全角文字の幅と同じ幅で表示されるようになります。

```
 $ patch -p1 < /path/to/diff/tmux-x.y-fix.diff
 (ここに、/path/to/diff は、 tmux-x.y-fix.diff が置かれたディレクトリのパス名)
```

また、[github 上の tmux の HEAD][TMRP] のソースコードにおける差分ファイルについても、 [github 上の tmux の HEAD][TMRP] のソースコードが置かれているディレクトリより、以下のようにして、最近の差分ファイルを適用後、 [tmux の HEAD 版][TMRP]を通常通りにビルドしてインストールすると、 [tmux][TMUX] において、 [East Asian Ambiguous Character][EAWA] が全角文字の幅と同じ幅で表示されるようになります。

```
 $ patch -p1 < /path/to/diff/tmux-HEAD-xxxxxxxx-fix.diff
 (ここに、 /path/to/diff は、 tmux-HEAD-xxxxxxxx-fix.diff が置かれたディレクトリのパス名)
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

## 使用条件

本差分ファイルは、端末多重化ソフトウェアである [tmux][TMUX] に適用する差分ファイルであり、 [waltarix 氏][WALT]と [Markus Kuhn 氏][DRMK]の両氏及び [Z.OOL. (mailto:zool@zool.jpn.org)][ZOOL] が著作権を有します。

従って、本スクリプトは [tmux][TMUX] のライセンスと同様である [ISC License][ISCL] に基づいて配布されるものとします。詳細については、本リポジトリに同梱する ```LICENSE``` を参照して下さい。

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
[ZOOL]:http://zool.jpn.org/
[ISCL]:https://www.isc.org/downloads/software-support-policy/isc-license/
