---
title: Linux_Ubuntu_窓使いの憂鬱をインストール
aliases:
  - Linux_Ubuntu_窓使いの憂鬱をインストール
tags:
  - d/2009/04/13
  - n/PGM/Linux/d/Ubuntu/v8.04
---

Ubuntu で窓使いの憂鬱使う方法 - 地獄の猫日記 http://d.hatena.ne.jp/nokturnalmortum/20090227/1235742723

↑に従ってやってみます。

単なる検証で自分ではなんもしてませんｗ


- 2009年04月13日
- Ubuntu 8.04 VMwareイメージ

どうしても統一動作がほしい
================================================================================

ダウンロード
================================================================================
窓使いの憂鬱 Linux & Mac (Darwin) 対応版とか配布してるところ http://members.at.infoseek.co.jp/hattoushin_uma/ ここからダウンロード

VMなんで適当にコピペしたけど

```
$ wget http://members.at.infoseek.co.jp/hattoushin_uma/cgi-bin/download/download.cgi/mayu-0.11.tar.gz
```

コマンドでゲットしてもいいですね

make
================================================================================
ユーザーのホームディレクトリで展開

```
$ tar xvzf mayu-0.11.tar.gz 
```

中に入って

```
$ cd mayu-0.11/
```

そんで設定関係ドン

```
$ ./configure
```

なんか

```
$ configure: error: We could not detect the boost libraries (version 1.33 or higher). 
```

`boost` ってライブラリの `1.33` 以上が必要なのよってことで入れるか・・・

コマンドドン

```
$ aptitude install libboost-dev libboost-dbg libboost-doc
```

`1.34` が入ったみたいだからこれでOKだろう

再び

```
$ ./configure
```

なんか成功

なので、定番の操作で

```
$ make
```

Windowsコンパイラとのオプションとか言語仕様の違いなのかしらんが、警告が何個かでるが、できたみたい
っで、ドン

```
$ sudo make install
```

ログを見る限り

```
/usr/local/bin/mayu
/usr/local/share/mayu
```

ここらにインストールされたようだ

起動
================================================================================
起動スクリプトを作る
--------------------------------------------------------------------------------
Ubuntu で窓使いの憂鬱使う方法 - 地獄の猫日記 http://d.hatena.ne.jp/nokturnalmortum/20090227/1235742723

ここからパクってきます・・・パクるといっても・・・定番の雛形ですけど

USER変数のところを、自分のユーザー名にしておきます。このファイルを `mayu` ってファイルで保存します

面倒なのでWindows側で作って、VM間のファイルコピペでUbuntu側にもっていこ・・・
↓以下に保存

```
/etc/init.d/
```

保存したヤツを実行できるようにしとく

```
$ sudo chmod +x mayu
```

なんか Windows で作ったファイルをVMの機能で持っていくと認識がおかしくなったので、ゼロから Ubuntu 側で作ったほうがいいかもね・・・

起動スクリプトを起動に組み込みます

```
$ sudo update-rc.d mayu start 10 2 . stop 10 0 1 3 4 5 6 .
```

通常は20番目あたりに追加されるんですけど明示的に10番目あたりにツッコムといいみたいです。

確認すると・・・
```
/etc/rc2.d$ ls -l
S10mayu -> ../init.d/mayu
```

確かに追加されている

キーの設定
================================================================================
これからがメインです。キーの設定をしましょう。
無変換キー＋exsdでのダイアモンドカーソル化が最終目的なので・・・
っでいきなり問題にぶち当たる・・・

> 日本語のキー名は使えない

なにおー！って思って

```
/user/local/share/mayu/linux109.mayu
```

を読んでみると

```
def key Muhenkan NonConvert                     =       0x5e 
```

こんな感じで書かれていたので、たぶん `Muhenkan` って名前で使えるのね

そんで

Greenbear Laboratory - 窓使いの憂鬱でEmacs風移動 http://mono.kmc.gr.jp/~yhara/w/?MayuEmacsLike

ここらを参考に・・・

こんな感じにしてみた

```
include "linux109.mayu"
keymap Global
mod mod0 = Muhenkan
key *S-M0-A = *S-Home
key *S-M0-F = *S-End
key *S-M0-D = *S-Right 
key *S-M0-S = *S-Left
key *S-M0-E = *S-Up
key *S-M0-X = *S-Down
```

このファイルを `.mayu` という名前でホームディレクトリに保存

最後にドン
================================================================================
自分のホームディレクトリに入れて、一応再起動してみる・・・動くか！？

うごかねーよ！ボケ！うぉぉぉぉ

って・・・

```
$ modprobe uinput
```

やったら動いた！　やったね

忘れないうちに `/etc/modules` ファイルに `uinput` と書いて起動時に読み込まれるようにしておきます。




