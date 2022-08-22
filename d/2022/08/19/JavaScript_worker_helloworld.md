---
title: JavaScript worker helloworld
aliases:
  - JavaScript_worker_helloworld
tags:
  - d/2022/08/19
  - n/PGM/JavaScript/Ope/ブラウザ操作/Web_Worker
---

Web Worker とはなんぞやというやつ。

基本形
================================================================================
このように作ってみた。

```javascript
document.addEventListener("DOMContentLoaded", function(e){
    const workerScript = `
        self.addEventListener("message", function(e){
            console.log("hello worker");
        });
    `;

    const blob = new Blob([workerScript], {type: 'text/javascript'})
    const myWorker = new Worker(URL.createObjectURL(blob))
    console.log("from index start");
    myWorker.postMessage({});
    console.log("from index end");
});
```

本来は別の JS ファイルを作ってそれを Worker のコンストラクタに食わせるのだが、1ファイルでやりたかったので、JavaScriptコードを String として作って(↑のテンプレート記法の中)それを Blob に変換してそのリソースの URL を生成して Worker を生成している。別ファイルにすれば `new Worker("hoge.js")` でいける。

そして開始メッセージを `console.log("from index start")` で表示する。

Worker 側では `addEventListener` で `message` イベントに処理をフックしている。
Worker のスクリプト内(テンプレート記法の部分)では `self` はそのグローバルコンテキストを指すようになるのでいきなり書いてあってもそれは `window` ではなく Worker 自身となる。なのでこの `addEventLister` している対象は、Worker のグローバルコンテキストそのままである。このイベントの対象は、メイン側で new された `myWorker` へ繋がっている。
同一オブジェクトでない、しかし見えないイベントで繋がっている。

new した Worker と、この Worker のスクリプトは別モノということを覚えておこう。

その後 `myWorker.postMessage({})` で `message` イベントを発生させている。
Object を渡しているのは、特に意味はなく引数が無いと怒られるからである。
先ほどフックした処理が動いて `hello worker` と表示する処理。

最後に `end` を表示。

これを実行すると、こうなる。

```
from index start
from index end
hello worker
```

そもそも Worker はブラウザの挙動を邪魔しないように別で重たい処理をやるための仕組みなので、当然ながら非同期なのである。

message を送る
================================================================================
さっきはイベントを発生させただけだが何かメッセージを送ってみる

このように

```javascript
myWorker.postMessage("hogehogehoge");
```


このように `e.data` としてイベントの `data` プロパティとして取り出せる。

```javascript
this.addEventListener("message", function(e){
    console.log("hello worker " + e.data);
});
```

結果は当然こうなる

```
from index start
from index end
hello worker hogehogehoge
```


結果を戻す
================================================================================
Worker は非同期処理なのだから、結果を受け取るのもコールバックということになる。

何に対してコールバックすればいいのか？ メインで new した Worker に対して同じように `message` イベントをフックすればよい。前述の通り new した Worker と Worker のスクリプトのコンテキストは別モノなので同じモノに同じイベントをフックしているのではない。

```javascript
myWorker.addEventListener("message", function(e){
    console.log("come back message -> " + e.data);
});
```

今度は Worker のスクリプト側から `message` イベントを起動する必要がある。
これも同様に `postMessage` になる。

```javascript
self.addEventListener("message", function(e){
    console.log("hello worker " + e.data);
    self.postMessage(e.data + "piyopiyopiyo");
});
```

ここで `self` に対して postMessage を呼び出しているのだがこのイベント発生に対しては自身でフックしているイベントは起動しない。前述のように別モノだから。

このイベントは new した側の Worker のイベントハンドラを起動する。

なので結果は

```javascript
document.addEventListener("DOMContentLoaded", function(e){
    const workerScript = `
        self.addEventListener("message", function(e){
            console.log("hello worker " + e.data);
            self.postMessage(e.data + "piyopiyopiyo");

        });
    `;

    const blob = new Blob([workerScript], {type: 'text/javascript'})
    const myWorker = new Worker(URL.createObjectURL(blob))
    myWorker.addEventListener("message", function(e){
        console.log("come back message -> " + e.data);
    });
    console.log("from index start");
    myWorker.postMessage("hogehogehoge");
    console.log("from index end");
});
```

こうなる。

```
from index start
from index end
hello worker hogehogehoge
come back message -> hogehogehogepiyopiyopiyo
```

これで大体の挙動はわかっただろう。スコープだけ気をつければ整理されたコーディングができそうだ。






























