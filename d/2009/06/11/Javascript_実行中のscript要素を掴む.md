---
title: Javascript 実行中のscript要素を掴む
aliases:
  - Javascript_実行中のscript要素を掴む
tags:
  - d/2009/06/11
  - n/PGM/JavaScript/Ope/ブラウザ操作
---

- 2009年06月11日

body要素中のscript要素
================================================================================
body要素中のscript要素。つまり画面構築時に実行されるJavascriptの場合は、document.writeメソッドが使えます。
document.writeされたものは要素として実行中の要素の直後に展開されます。しかもこの展開はdocument.writeメソッドの実行直後に行われます。

こいつを利用して適当なランダム値のidを生成しそれを divとかの適当な要素にくっつけてdocument.write その直後にgetElementByIdでその要素を掴んで　その要素の直前兄弟ノードをpreviousSiblingプロパティで取得すると・・・自分の要素がつかめます。

head要素中のscript要素
================================================================================
画面構築後に後付されるscript要素はだいたhead要素内にぶら下げることになる。

ここで問題になるのはdocument.writeを使うとおそらくバグるということだ・・・

・・・どうしようか・・・現状未解決
