---
title: JavaScript jsdom を使ってみる
aliases:
  - JavaScript_jsdom_を使ってみる
tags:
  - d/2022/08/25
  - n/PGM/JavaScript/Node.js/jsdom
  - n/PGM/JavaScript/Node.js/テスト
---


ブラウザ無しにブラウザ的な挙動を確かめたい場合に使うやつ。

- Windows10
- node v16.15.0

Install
================================================================================
なんでもよい

```
$ npm install jsdom
```


サンプルコード準備
================================================================================
`hoge.js` ファイルを用意して

```javascript
console.log("hello jsdom");
```

実行

```
$ node hoge.js
hello jsdom
```

OK


使ってみる
================================================================================

まず対象の `Piyo` クラス

```javascript
const Piyo = class{
    constructor(){
        this.domButton = document.querySelector("#piyoButton");
        this.domButton.addEventListener("click", ()=>{
            this.dododo();
        });
    }
    dododo(){
        console.log("dododo in piyo");
    }
};
module.exports = Piyo; 
```

どこでもありそうなやつ。コンストラクタで画面から DOM を掴んで保持して、それにイベント引っかけて、その処理は処理でクラス内のハンドラを使うみたいなやつ。

ポイントは、DOM を掴む時に `document.querySelector` を使っていること。
これはブラウザの API なので node 側は持ってない。

最後に `exports` を足して node 側で引き込めるようにしてある。
このコードは定義の最後に書いてあるので、もしブラウザで開いたとしても、この1行がミスるだけで全体としては動作する。


```javascript
const { JSDOM } = require("jsdom");
const Piyo = require("./piyo.js");

const htmlCode = `
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
        <title>ほげほげのページ</title>
        <meta name="description" content="説明文" />
    </head>
    <body>
        <button id="piyoButton">ぼたん</button>
 
    </body>
</html>
`;

const dom = new JSDOM(htmlCode);
globalThis.document = dom.window.document;
const piyo = new Piyo();

const domBtn = document.getElementById("piyoButton");
domBtn.click();
```


クラス `Piyo` が使われる想定であるようなブラウザへの HTML をテンプレート記法で記述しそれを元に `JSDOM` を生成するとそれっぽいモノを生成してくれる(完全一致ではない)。

ここで出来上がった、`dom` からお馴染みの `window` や `document` オブジェクトを取得することができる。

`Piyo` のコンストラクタ内部で `document.querySelector` を使えるようにしておく必要がある。
ブラウザのグローバルオブジェクトは `window` なのであるが、node のグローバルオブジェクトは `global` ということになっている。この差異を吸収する記述として `globalThis` というものがある。


今回はこれを使って `document` を使えるようにする。 `jsdom` から取得できる `document` を `globalThis` の `document` として入れ込む。

これで `Piyo` のコンストラクタ内で使われる `document` は `dom.window.document` を指すことになるので `querySelector` が可能になる。

DOM のボタンを取得してそいつをクリックすると、Piyo で定義されているハンドラの処理が発火することが確認できる。

ブラウザがまったく無くても、ブラウザで使う前提のコードが node で駆動して動作確認できる。







