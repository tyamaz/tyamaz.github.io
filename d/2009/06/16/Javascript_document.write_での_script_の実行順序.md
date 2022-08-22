---
title: Javascript_document.write_での_script_の実行順序
aliases:
  - Javascript_document.write_での_script_の実行順序
tags:
  - d/2009/06/16
  - n/PGM/JavaScript
---


- 2009年06月16日
- Firefox 3.0.11

memo
================================================================================
順序が変？
--------------------------------------------------------------------------------
head 要素内の script 要素で `document.write` メソッドを使ってさらに `script` 要素を作った場合実行順序は、

`document.write` したスクリプト → `document.write` されたスクリプトになる。

・・・っが alert でやった場合だと微妙に逆になったりする・・・なんだこれ
