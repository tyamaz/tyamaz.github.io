---
title: GNU Screen
aliases:
  - GNU_Screen
tags:
  - d/2010/10/04
  - n/PGM/Linux/GNU_Screen
---

- 2010-10-04

ここ超参考
================================================================================
http://www.dekaino.net/screen/

Screenとは
================================================================================
挙動的には高機能なCUI版ターミナルエミュレータと思ってOK

設定
================================================================================
設定ファイル
--------------------------------------------------------------------------------
設定ファイルはホームディレクトリに

```
.screenrc
```

ファイルを作って行う

Screen自体に命令を送り込むキーバインド変更
--------------------------------------------------------------------------------
`.screenrc` ファイルに

```
escape ^z^z
```

と書いておくと「`ctrl + z`」で Screen に命令を送り込めるようになります
以下全部 `ctrl + z` で送り込む前提で書きます。


操作
================================================================================
新しい仮想画面の作成
--------------------------------------------------------------------------------

```
ctrl + z
c
```

直前の仮想画面への移動
--------------------------------------------------------------------------------

```
ctrl + z
space
```

仮想画面の一覧表示
--------------------------------------------------------------------------------

```
ctrl + z
w
```

仮想画面の直接移動（番号指定）
--------------------------------------------------------------------------------
仮想画面の一覧表示で一覧に仮想画面の番号が出るのでそれを指定すると直接移動できる

```
ctrl + z
番号
```

キーの関係上 0 から 9 の 10 枚しか使えないが十分だろう



