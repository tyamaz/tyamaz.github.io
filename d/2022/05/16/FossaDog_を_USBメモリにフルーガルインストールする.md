---
title: FossaDog_を_USBメモリにフルーガルインストールする
aliases:
  - FossaDog_を_USBメモリにフルーガルインストールする
tags:
  - d/2022/05/16
---


Windows10 ホスト 上の Linux Mint 20.1

ホストに刺した USB メモリを Linux 側にマウントする。

Linux Mint に USB メモリフォーマッタ というツールがあるのでそれで ext4 で全部フォーマットする
そこに `FossaDog` という名前のディレクトリを1個作っておく

FossaDog の ISO イメージをマウントして

![](public/d/2022/05/16/Pasted%20image%2020220525113446.png)

この中に `casper` というディレクトリがあるのでこれを丸ごとフォーマットした USB メモリの `FossaDog` にコピーする

