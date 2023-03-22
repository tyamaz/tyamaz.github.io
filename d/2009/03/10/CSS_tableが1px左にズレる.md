---
title: CSS tableが1px左にズレる
aliases:
  - CSS_tableが1px左にズレる
tags:
  - d/2009/03/10
  - n/PGM/CSS
  - n/PGM/browser/Firefox/v2
  - n/PGM/browser/Firefox/v3
---


テーブルがなぜか1px左にずれる。これはバグなのか仕様なのかなんかはしらんけど Firefox2 で発生してたのでメモ


追記：2009年03月10日: Firefox3でも発生、なにやら table を div で囲んでごにょごにょすると発生するっぽ

```html
<div>
    <h1></h1>
    <table>
    </table>
</div>
```

こういう風に配置してtableに

```css
table{
    border-collapse:collapse;
}
```

こうやって指定しておくと
そうするとなぜか table の左端が 1px だけ h1 のブロックよりも左側に飛び出ます。
