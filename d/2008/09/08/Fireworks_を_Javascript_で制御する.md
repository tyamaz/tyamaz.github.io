---
title: Fireworks_を_Javascript_で制御する
aliases:
  - Fireworks_を_Javascript_で制御する
tags:
  - d/2008/09/08
  - n/PGM/Fireworks/MX2004
  - n/PGM/JavaScript/etc
---


- 2008年09月08日
- Fireworks MX 2004 (ver7) Windows版

jsfファイル
================================================================================
Fireworks 用の Javascript ファイルは `jsf` というファイルに記述するようです。


オブジェクト
================================================================================
fwオブジェクト
--------------------------------------------------------------------------------
Firework アプリをあらわすオブジェクトっぽい。
通常の Javascript でいえば window オブジェクトにあたるみたいなものか


documentオブジェクト
--------------------------------------------------------------------------------
現在開いているファイルを表すオブジェクト

```javascript
var doc = fw.getDocumentDOM();
```


こんな感じで取得できる


プロパティ
================================================================================
幅
--------------------------------------------------------------------------------

