---
title: CSS_td要素にpositionは通じない
aliases:
  - CSS_td要素にpositionは通じない
tags:
  - d/2009/03/25
  - n/PGM/browser/Firefox/v3.0.7
  - n/PGM/browser/IE/v7
---

- 2009年03月25日
- Firefox3.0.7
- IE7

td要素にpositionのスタイルは設定できない。Firefoxではできない、だからセルに

```css
position:relative;
```

指定して、セル中に

```css
position:absolute;
```

やって自由にやろうとするとぶっ飛ぶ

div か何かを入れ子にして回避すりゃいいかな
