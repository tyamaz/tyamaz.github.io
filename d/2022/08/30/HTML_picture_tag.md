---
title: HTML picture tag
aliases:
  - HTML_picture_tag
tags:
  - d/2022/08/30
  - n/PGM/HTML
---


HTML の `picture` タグ

画像を示すなら通常は `img` タグだが、それに対して複数のオプションを付けられるというタグ。

["picture" \| Can I use\.\.\. Support tables for HTML5, CSS3, etc](https://caniuse.com/?search=picture) 使用に関して特に問題はなさそう。

このように使う

```html
<picture>
    <source srcset="200x300.jpg" media="(max-width: 300px)">
    <source srcset="400x600.jpg" media="(max-width: 600px)">
    <img src="500x700.jpg">
</picture>
```

これにより、3つの画像を画面サイズ別に出し分けることができる。
通常このような処理は CSS で行っていたが、HTML 側でやることで通信を省略できるようになる。

個人的には見た目の部分が CSS と分散してしまうので嬉しくないように感じる。
解像度違いでは使わないほうがよいだろう `img` に `srcset` 属性があるのでそっちを使うのが筋だろう。

表示条件としてはより上に書いたほうが優先される。

しかし、それは表示条件に対してだけで、表示できるか否かではない。
なので `200x300.jpg` が存在しなかった場合、表示されないという表示になる `400x600.jpg` が繰り上がらない。

表示されているのはあくまで `img` 要素であって `source` 要素はそのパラメータを提供しているという体になる。`source` 要素が表示されているわけでない。

なので

```css
img{
    display:none;
}
```

とすると全部表示されなくなる。

```css
source{
    display:none;
}
```

としても、別に普通に表示される。




