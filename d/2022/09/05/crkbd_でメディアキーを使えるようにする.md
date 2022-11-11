---
title: crkbd_でメディアキーを使えるようにする
aliases:
  - crkbd_でメディアキーを使えるようにする
tags:
  - d/2022/09/05
  - n/PGM/キーボード/crkbd
---

defalut の keymap をコピーしている前提。

ここに基本設定がある。

```
qmk_firmware/keyboards/crkbd/rules.mk
```

この中にこのような行がある。

```c
EXTRAKEY_ENABLE = no       # Audio control and System control
```

コイツをコピーする。

そして

自分のキーマップのディレクトリの `rules.mk` にこの一行を追加し、設定値を `yes` に書き換える。

```
EXTRAKEY_ENABLE = yes # Audio control and System control
```

ビルドしてインストールする。

この `rules.mk` 系の設定を有効にすると、ビルドしたサイズが増加するので、ビルドが失敗する可能性もある。
今回それを `yes` にしたので、ビルド時にこのような警告が出た。

```
Checking file size of crkbd_rev1_legacy_ore.hex
                            [WARNINGS]

 * The firmware size is approaching the maximum - 27876/28672 (97%, 796 bytes free)
```







