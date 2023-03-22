---
title: Javascript textarea 内の改行の扱い
aliases:
  - Javascript_textarea_内の改行の扱い
tags:
  - d/2009/03/25
  - n/PGM/JavaScript/Ope/ブラウザ操作/DOM
  - n/PGM/JavaScript/Ope/文字列操作
---

2009年03月25日

textarea内の改行の扱いがIEとFirefoxで違う
================================================================================
IEは `\r\n` Firefoxは `\n` でハンドリングできる。


改行コードを変えて処理する
================================================================================

```javascript
hoge = hoge.replace(/\r\n/g, "\n");
```

こんな風にして統一してから処理する


