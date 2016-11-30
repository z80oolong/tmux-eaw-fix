# tmux 2.3 において East_Asian_Width 特性が A である文字を全角文字の幅で表示する

tmux 2.3 において、 Unicode の規格における東アジア圏の各種文字のうち、いわゆる "◎" や "★" 等の記号文字及び罫線文字等、 East_Asian_Width 特性の値が A (Ambiguous) となる文字 (以下、 East_Asian_Width 特性が A の文字) が、日本語環境で文字幅を適切に扱うことが出来ずに表示が乱れる問題が発生しています。

このファイルは、 tmux 2.3 において East_Asian_Width 特性が A の文字の幅を漢字や全角カナ文字等と同じ幅 2 で表示するように修正するための差分ファイルです。

tmux 2.3 のソースコードが置かれているディレクトリに移動した後、以下のようにして差分ファイル tmux-2.3-fix.diff を適用します。

```
# patch -p1 < /path/to/diff/tmux-2.3-fix.diff
(ここに、/path/to/diff は、 tmux-2.3-fix.diff が置かれたディレクトリのパス名)
```

差分ファイルを適用後、 tmux 2.3 を通常通りに build してインストールすると、 tmux 2.3 において、 East_Asian_Width 特性が A の文字が全角文字の幅と同じ幅で表示されるようになります。

また、 East_Asian_Width 特性が A の文字の文字幅を 2 ではなく 1 として扱う場合は、tmux の設定ファイル .tmux.conf に以下の設定を追記します。

```
set-option -g utf8-cjk off
```

なお、この差分ファイルを作成するに当たっては、下記の URL にある、 Markus Kuhn 氏が作成した East_Asian_Width 特性が A の文字の扱いを考慮した wcwidth(3) 関数の実装を使用しました。 Markus Kuhn 氏には心より感謝いたします。

[http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c](http://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c)