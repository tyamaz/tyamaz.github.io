---
title: Javascript_String.split
aliases:
  - Javascript_String.split
tags:
  - d/2009/02/05
  - d/PGM/JavaScript/Ope/文字列操作
---



- 2009年02月05日
- Firefox3.0.6

空白で分割する
================================================================================
```javascript
var a = "a b c d e";
a.split(" "); //=> ["a", "b", "c", "d", "e"];
```

二つ挟み

```javascript
var a = "a b  c d e";
a.split(" "); //=> ["a", "b", "", "c", "d", "e"];
```

空

```javascript
var a = "";
a.split(" "); //=> [""];
```

