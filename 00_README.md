# tmux 2.3 において East Asian Width 特性が Ambiguous である文字を全角文字の幅で表示する

tmux 2.3 において、 Unicode の規格における東アジア圏の各種文字のうち、いわゆる "◎" や "★" 等の記号文字及び罫線文字等、 East_Asian_Width 特性の値が A (Ambiguous) となる文字が、日本語環境で文字幅を適切に扱うことが出来ずに表示が乱れる問題が発生しています。

このファイルは、 tmux 2.3 において East Asian Width 特性が Ambiguous である文字の幅を漢字や全角カナ文字等と同じ幅 2 で表示するように修正するための差分ファイルです。

tmux 2.3 のソースコードが置かれているディレクトリに移動した後、以下のようにして差分ファイル tmux-2.3-utf8cjk.diff を適用します。

```
# patch -p1 < /path/to/diff/tmux-utf8cjk.diff
(ここに、/path/to/diff は、 tmux-utf8cjk.diff が置かれたディレクトリのパス名)
```
