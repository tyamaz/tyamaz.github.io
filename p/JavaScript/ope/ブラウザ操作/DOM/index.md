---
title: JavaScript / ブラウザ操作 / DOM
aliases:
  - DOM
  - JavaScript / ブラウザ操作 / DOM
---



このページの内容は http://yakinikunotare.boo.jp/orebase2/javascript/dom_ope に移動しました

*目次 [#u9fdf8f3]
#contents

*バージョンと製造年月日 [#pd1b84a4]
2009年01月17日


*すべてのノードを取得する [#z428ce76]
#geshi(javascript){{
document.getElementsByTagName("*");
}}


*子要素を削除する [#tea96e8a]
子要素は
#geshi(javascript){{
hogeparent.removeChild(hogechild);
}}
で削除する。これはオブジェクトをあくまで親要素から引き剥がすだけであって、GCによってオブジェクトを破壊するわけではない

*オブジェクトを画面に再描画させる [#ob0130d4]
#geshi(javascript){{
var hoge = document.createElement("img");
document.body.appendChild(hoge);
document.body.appendChild(hoge);

}}
2つのオブジェクトが表示される直感だが、1個しか出ない。DOMは画面上に1個しか存在できない。

先に存在するほうが優先されて、後の変更は無視される。
これはDOMをJavascriptのオブジェクトでラッピングして操作する際にバグになりやすい。

画面表示のためにDOMをハンドリングするときは常に引き剥がす処理を先行して行わなければ、後続の貼り付ける処理で何も起きなくなる。

*直前に挿入 [#ud16df95]
#geshi(javascript){{
親ノード.insertBefore(前に挿入したいヤツ, 挿入されるやつ);
}}

*交換 [#a957a060]
実際には交換ではなくて交換される側がremoveChildされる。つまり画面に貼り付けられた2要素を交換すると片方が消えてしまう。
#geshi(javascript){{
親ノード.replaceChild(入れたいやつ, 交換されるやつ);
}}
*参考サイト [#k3dc888d]
#rakutendad

*タグ [#b72dcb2c]
#lsx(tag=Javascript^DOM)
&tag(Javascript,DOM,移動済み);
