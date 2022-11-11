---
title: Javascript unload イベント
aliases:
  - Javascript_unload_イベント
tags:
  - d/2008/09/17
  - n/PGM/JavaScript/Ope/ブラウザ操作/イベント/unload
  - n/PGM/browser/Firefox/v3.0
---

- 2008年09月17日
- Firefox3.0



発生順序
================================================================================

- submit ボタン click
- form 要素 submit
- window 要素 unload

記述箇所
================================================================================
HTMLでは onunload イベントハンドラを持っているのは body 要素になる
Javascript では window オブジェクトに持っている

