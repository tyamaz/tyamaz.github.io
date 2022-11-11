---
title: JavaScript contenteditable 要素の選択範囲を増減させる
aliases:
  - JavaScript_contenteditable_要素の選択範囲を増減させる
tags:
  - d/2022/09/12
  - n/PGM/JavaScript/Ope/ブラウザ操作
---

選択範囲をコントロールする例として `input text` や `textarea` はよく出てくるが、`contenteditable` 要素や通常の要素に関してはあまり解説が無いので調べてみた。

- Windows10
- Google Chrome v105


こういうやつ。

```html
<div contenteditable="true">This is a pen</div>
```


これの選択範囲というのは、このようにグローバルに取れる

```javascript
const sel = window.getSelection();
```

この `Selection` オブジェクトからイロイロ取れたり操作したりできる。

その選択している文字列を取りたいならこれでよい。

```javascript
const sel = window.getSelection();
console.log(sel.toString());
```

選択範囲増減させるには `modify` というメソッドを使う。
このメソッドはやや特殊で、直接的に選択範囲を変えるのではなく、変える基準を提示して、変更する。

選択範囲を1文字文狭めたいなら、こうなる。

```javascript
sel.modify("extend", "left", "character");
```

`extend` 範囲変更、`left` 左側へ、`character` 文字単位で、という指示になる。
2文字ズラしたいなら、2回呼び出せばよい。

`extend` の他にもカーソル移動を示す `move`、`left` があるなら当然 `right` もある、

`character` 以外にも。

- character
- word
- sentence
- line
- paragraph
- lineboundary
- sentenceboundary
- paragraphboundary
- documentboundary

こういう指定ができる。










