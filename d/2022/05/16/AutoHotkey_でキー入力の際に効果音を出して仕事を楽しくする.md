---
title: AutoHotkey_でキー入力の際に効果音を出して仕事を楽しくする
aliases:
  - AutoHotkey_でキー入力の際に効果音を出して仕事を楽しくする
tags:
  - d/2022/05/16
---


このようにする。


```ahk
~^c:: SoundPlay,hadoken.wav
~^v:: SoundPlay,shoryuken.wav
```


これでコピペの際に「波動昇竜」が出るようになる。

頭に `~` チルダをつけると、キー入力をトラップしないで、そのままシステムにスルーするようになる。
