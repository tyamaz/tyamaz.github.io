---
title: CSS 画面下までいっぱいのブロックを作る
aliases:
  - CSS_画面下までいっぱいのブロックを作る
tags:
  - d/2008/03/01
  - n/PGM/CSS
---


```css
html{
    height:100%;
}
body{
    height:100%;
}
#bodychokka{
    height:100%;
}
```

id `bodychokka` は `body` の最初の子要素のブロック

こう CSS で指定すると `bodychokka` ボックスが画面下まで伸びる

※ 画面いっぱい系 CSS を使う場合は HTML 構造も歪になるし、ブラウザ間での挙動を調整するのも骨が折れるので、そういう実装にしないか、JavaScript で解決することをオススメします（2008年3月時点）

※ 2022年現在、要素の縦方向を外部ブロックいっぱいにする記述は普通にサポートされている

