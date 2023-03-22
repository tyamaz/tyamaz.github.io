---
title: ActionScript3.0 TextFieldで文字サイズを変える
aliases:
  - ActionScript3.0_TextFieldで文字サイズを変える
tags:
  - d/2022/11/22
  - n/PGM/ActionScript/v3.0
  - n/PGM/Flash/CS3
---

- 2009年02月07日
- ActionScript3.0
- Flash CS3

文字サイズを指定する
================================================================================
pt単位で指定する。デフォルトは 12pt

```actiohscript
TextFormat.size = 100;
```


文字サイズを指定する最大
================================================================================
なぜか文字サイズの限界が 127pt

```actiohscript
TextFormat.size = 127;
```

文字サイズを限界を超えて指定する
================================================================================

```actiohscript
TextFormat.size = 127;
```

した後

```actiohscript
hoge.scaleX = 2.0;
hoge.scaleY = 2.0;
```

これで256ptの文字サイズになる


