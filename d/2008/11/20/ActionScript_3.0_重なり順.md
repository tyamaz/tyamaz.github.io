---
title: ActionScript3.0_重なり順
aliases:
  - ActionScript3.0_重なり順
tags:
  - d/2008/11/20
  - n/PGM/ActionScript/v3.0
  - n/PGM/Flash
---

- 2008年11月20日
- ActionScript3.0

ActionScript3.0でのインスタンスの重なり順
================================================================================
インスタンスの重なり順。`z-index` とか `z-order` とか呼ばれるようなやつ。

ActionScript3.0 では 2.0 と違って、インスタンス個々が重なり順を管理しない。

親オブジェクトに貼り付けた順で重なり順を管理している。
つまり重なり順は親オブジェクトの管理下にある

なのでプロパティ「`z`」の意味合いは変わっているので注意。

- `setChildIndex`
- `swapChildren`
- `swapChildrenAt`

などのメソッドで制御可能

