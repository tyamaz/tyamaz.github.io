---
title: Flex 3.0 MXML と見た目の分離
aliases:
  - Flex_3.0_MXML_と見た目の分離
tags:
  - d/2009/02/28
  - n/PGM/Flex/v3.0
---

- 2009年02月28日
- Flex3.0

見た目の分離
================================================================================
HTML+CSSと同じような関係でMXML+CSSで構造と見た目を分離することができる。

```xml
<?xml version="1.0" ?>
<mx:Application xmlns:mx="http://www.adobe.com/2006/mxml">
  <mx:Style source="hoge.css" />
  <mx:TextArea id="hello" styleName="hello" text="はろーわーるど" />
</mx:Application>
```

```css
Application{
    background-color:#000000;
}
.hello{
    border-color:#FF0000;
}
```

StyleタグでCSSファイルを読み込んだ。
スタイルは今回はタグセレクタで全体の背景を黒にして、クラスセレクタでテキストの周りを赤に線を引いてみた。

idセレクタとかはないっぽいし、使えるプロパティも少ない。ここら辺はHTMLのCSSのほうが高性能

