---
title: HTML / Flash要素の上に他の要素を重ねる方法
aliases:
  - HTML / Flash要素の上に他の要素を重ねる方法
tags:
  - d/2009/02/07
  - n/old
  - n/PGM/Firefox/2
  - n/PGM/Firefox/3_0_6
  - n/PGM/IE/7
---




Flash の z-index がコントロールできない
================================================================================
`CSS` の `z-index` 設定すればいいじゃんと思うが `z-index` を設定しても `Flash` の `object要素`の上に他の要素をもってくることはできない。なぜか `Flashの要素` が上にきてしまう。

wmode属性の指定
================================================================================
`Firefox` では `embed要素` に

```
wmode="transparent"
```

`wmode属性` を加えてやると `z-index` に従うようになる・・・意味不明。

`IE7` では、`Object要素の子要素` に `wmode` を与えてやると動く

```xml
<param name="wmode" value="transparent"/>
```

