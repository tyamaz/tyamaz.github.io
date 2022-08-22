---
title: JavaScript で画像をAA化するプログラムを作る
aliases:
  - JavaScript_で画像をAA化するプログラムを作る
tags:
  - d/2022/07/25
---

特に深い意味はなく、適当な知識を適当にかき集めて適当にそれっぽいモノが実装できるのかの話


ザックリ方針イメージ
================================================================================
- なんらかの方法で画像のデータを JS へ取り込む
- canvas に描画する
- ピクセルデータを抽出する
- 何か前処理をする
- ピクセルデータから文字データに変換する
- 描画する

10分間適当にググったらこのような流れでできそうな気がする気がする。


なんらかの方法で画像データを JS へ取り込む
================================================================================
お手軽にクリップボードから入れ込むことにする。

まず適当に HTML と JS を用意して駆動するようにする。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
        <title>ほげほげのページ</title>
        <meta name="description" content="説明文" />
        <script src="index.js"></script>
        <script>
            document.addEventListener("DOMContentLoaded", function(e){
            });
        </script>
        <style>
            #pastearea{
                width: 50px;
                height: 50px;
                background-color: red;
                color: white;
            }
            #view{
                width: 200px;
                height: 200px;
                background-color: blue;
            }
        </style>
    </head>
    <body>
        <div id="pastearea">paste here</div>
        <div id="view"></div>
    </body>
</html>
```


![](Pasted%20image%2020220725165524.png)


適当にやるとこうなる。


クリップボードからデータを取得する
================================================================================
ペーストのイベントにフックしてそこから値を引き出せばよいらしい。


```javascript
class MyImage{
    constructor(_d){
        this.d = _d;
    }
    toDomImg(){
        const domImg = document.createElement("img");
        domImg.src = this.toUrl();
        return domImg;
    }
    toUrl(){
        return URL.createObjectURL(this.d);
    }
}

class MyApp{
    constructor(){
        this.domPasteArea = document.querySelector("#pastearea");
        this.domView = document.querySelector("#view");
        this.domPasteArea.addEventListener("paste", (e) => {
            this.pasteImageHandler(e);
            e.preventDefault();
        });
    }
    pasteImageHandler(e){
        for(let item of e.clipboardData.items){
            if(item.type == "image/png"){
                this.myImage = new MyImage(item.getAsFile());
            }
        }
        if(this.myImage != null){
            this.domView.appendChild(this.myImage.toDomImg());
        }
    }
}
```


`paste` イベントの `clipboardDate` から `item` を引き出してその `type` が画像だったら、`view` に要素追加するという処理をしている。

やってみると動作したので、これでクリップボードからの画像データの取り出しはできることがわかった。


canvas に描画する
================================================================================
データは取り出せてそれを DOM で描画できたので、今度はそれを `canvas` に描画する。

`canvas` は HMTL 要素なので埋め込んでおく

```html
<canvas id="mycanvas" width="300" height="300"></canvas>
```


それからスーパー雑に検証するとこうなる。

```javascript
class MyImage{
    constructor(_d){
        this.d = _d;
    }
    toDomImg(){
        const domImg = document.createElement("img");
        domImg.src = this.toUrl();
        return domImg;
    }
    toUrl(){
        return URL.createObjectURL(this.d);
    }
}

class MyApp{
    constructor(){
        this.domPasteArea = document.querySelector("#pastearea");
        this.domView = document.querySelector("#view");
        this.domCanvas = document.querySelector("#mycanvas");
        this.domPasteArea.addEventListener("paste", (e) => {
            this.pasteImageHandler(e);
            e.preventDefault();
        });
    }
    pasteImageHandler(e){
        for(let item of e.clipboardData.items){
            if(item.type == "image/png"){
                this.myImage = new MyImage(item.getAsFile());
            }
        }
        if(this.myImage != null){
            const domImg = this.myImage.toDomImg();
            this.domView.appendChild(domImg);
            const ctx = this.domCanvas.getContext("2d");
            domImg.addEventListener("load", ()=>{
                ctx.drawImage(
                        domImg,
                        0, 0,
                        domImg.naturalWidth, domImg.naturalHeight,
                        0, 0,
                        this.domCanvas.width, this.domCanvas.height);
            });
        }
    }
}
```


img タグでの描画と canvas タグでの描画ができた。

ポイントは 画像の `load` イベントに引っかけて描画するということっぽい。
これをそのままイベントにフックせずに実行すると確かに描画されないのだ

↓こうでは描画されない。

```javascript
const domImg = this.myImage.toDomImg();
this.domView.appendChild(domImg);
const ctx = this.domCanvas.getContext("2d");
ctx.drawImage(
        domImg,
        0, 0,
        domImg.naturalWidth, domImg.naturalHeight,
        0, 0,
        this.domCanvas.width, this.domCanvas.height);
```


が、これなんか怪しい。そういう例しか出てこないのでそう書いてみたのだが、この img はデータが全部 URL に書き込まれていてロードするというタイミングは存在しないからである。

試しにこう書いてみると動かない。

```javascript
const domImg = this.myImage.toDomImg();
this.domView.appendChild(domImg);
const ctx = this.domCanvas.getContext("2d");

setTimeout(()=>{
    domImg.addEventListener("load", ()=>{
        ctx.drawImage(
                domImg,
                0, 0,
                domImg.naturalWidth, domImg.naturalHeight,
                0, 0,
                this.domCanvas.width, this.domCanvas.height);
    });
}, 3000);
```


イベントへのフックを3秒遅延させてみた。そうするとフックするときにはもうイベントは終わっているので、
後でフックしても動かないという流れである。

つまり最初の load フック版の処理がなぜうまくいって、そのまま書いた版がなぜ動かないかは

- load フック版
  - img の src に値の設定 ※ ここで JS 処理と ブラウザ処理の並列処理になる
  - JS処理 ブラウザ処理とは別なので画面描画を待たずに load のフック処理に入る
  - ブラウザ処理 src に基づき img を描画しようとする
  - load イベント発生
  - フックした処理が動いて canvas が描画される
- そのまま書いた版
  - img の src に値の設定 ※ ここで JS 処理と ブラウザ処理の並列処理になる
  - ブラウザ処理 src に基づき img を描画しようとする 描画終わってない
  - JS処理 ブラウザ処理とは別なので画面描画を待たずに canvas を描画しにいく
  - img がまだ無いので描画するモノが無い
  - canvas が描画されない

処理の微妙な速度差の影響でなんとなくうまくいっているという処理。

なので雑にタイミングを調整してやると...

```javascript
const domImg = this.myImage.toDomImg();
this.domView.appendChild(domImg);
const ctx = this.domCanvas.getContext("2d");

setTimeout(()=>{
    ctx.drawImage(
            domImg,
            0, 0,
            domImg.naturalWidth, domImg.naturalHeight,
            0, 0,
            this.domCanvas.width, this.domCanvas.height);
}, 3000);
```

これだと予想通りうまくいく。

つまり、作るべきはコールバックの仕組み。
そんなに深いコールバックでもないので Promise 実装にするまでもないかな。

結果こうなる

```javascript
class MyImage{
    constructor(_d){
        this.d = _d;
    }
    toDomImg(_callBack){
        const domImg = document.createElement("img");
        // src より先のタイミングでフックする
        if(_callBack != null){
            domImg.addEventListener("load", ()=>{
                callBack(domImg);
            });
        }
        domImg.src = this.toUrl();
        return domImg;
    }
    toUrl(){
        return URL.createObjectURL(this.d);
    }
}

class MyApp{
    constructor(){
        this.domPasteArea = document.querySelector("#pastearea");
        this.domView = document.querySelector("#view");
        this.domCanvas = document.querySelector("#mycanvas");
        this.domPasteArea.addEventListener("paste", (e) => {
            this.pasteImageHandler(e);
            e.preventDefault();
        });
    }
    pasteImageHandler(e){
        for(let item of e.clipboardData.items){
            if(item.type == "image/png"){
                this.myImage = new MyImage(item.getAsFile());
            }
        }
        if(this.myImage != null){
            const domImg = this.myImage.toDomImg((_img)=>{
                const ctx = this.domCanvas.getContext("2d");
                ctx.drawImage(
                        _img,
                        0, 0,
                        _img.naturalWidth, domImg.naturalHeight,
                        0, 0,
                        this.domCanvas.width, this.domCanvas.height);
            });
            this.domView.appendChild(domImg);
        }
    }
}
```


ネット上の情報も怪しいし、コードもやや注意する必要があった。



canvas からピクセル情報を得る
================================================================================
canvas に描画できたのでそこからピクセル情報を採る。

これで取れるっぽい。
左上から 1px 分の情報が欲しいならこうだ。

```
const hoge = ctx.getImageData(0, 0, 1, 1);
```


実際取ると、このような情報が手に入る。

```
ImageData {data: Uint8ClampedArray(4), width: 1, height: 1, colorSpace: 'srgb'}
data: Uint8ClampedArray(4)
0: 0
1: 0
2: 0
3: 255
buffer: ArrayBuffer(4)
byteLength: 4
byteOffset: 0
length: 4
Symbol(Symbol.toStringTag): "Uint8ClampedArray"
[[Prototype]]: TypedArray
colorSpace: "srgb"
height: 1
width: 1
```

今回は真っ黒画像にした。その data というキーに大きさ4の配列的な何かがあって、上から順におそらく RGBアルファチャンネルという値がセットされている。


範囲で取った場合ってどうなる？

```
const hoge = ctx.getImageData(0, 0, 2, 2);
```

この場合は2次元データが1次元に展開されて、(0,0),(0,1),(1,0),(1,1) と大きさ 16 の配列として表現される。


ではその情報を元に canvas に書き込んでみる。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
        <title>ほげほげのページ</title>
        <meta name="description" content="説明文" />
        <script src="index.js"></script>
        <script>
            document.addEventListener("DOMContentLoaded", function(e){
                const app = new MyApp();
            });
        </script>
        <style>
            #pastearea{
                width: 50px;
                height: 50px;
                background-color: red;
                color: white;
            }
        </style>
    </head>
    <body>
        <div id="pastearea">paste here</div>
        <canvas id="mycanvas1" width="200" height="300"></canvas>
        <button id="dododobutton">dododo</button>
        <canvas id="mycanvas2" width="200" height="300"></canvas>
    </body>
</html>

```



```javascript
class MyImage{
    constructor(_d){
        this.d = _d;
    }
    toDomImg(_callBack){
        const domImg = document.createElement("img");
        // src より先のタイミングでフックする
        if(_callBack != null){
            domImg.addEventListener("load", ()=>{
                _callBack(domImg);
            });
        }
        domImg.src = this.toUrl();
        return domImg;
    }
    toUrl(){
        return URL.createObjectURL(this.d);
    }
}

class MyApp{
    constructor(){
        this.domPasteArea = document.querySelector("#pastearea");
        this.domCanvas1 = document.querySelector("#mycanvas1");
        this.domCanvas2 = document.querySelector("#mycanvas2");
        this.dododoButton = document.querySelector("#dododobutton");

        this.domPasteArea.addEventListener("paste", (e) => {
            this.pasteImageHandler(e);
            e.preventDefault();
        });
        this.dododoButton.addEventListener("click", ()=>{
            this.convertImage();
        });
    }
    pasteImageHandler(e){
        for(let item of e.clipboardData.items){
            if(item.type == "image/png"){
                this.myImage = new MyImage(item.getAsFile());
            }
        }
        if(this.myImage != null){
            const domImg = this.myImage.toDomImg((_img)=>{
                const ctx = this.domCanvas1.getContext("2d");
                ctx.drawImage(
                        _img,
                        0, 0,
                        _img.naturalWidth, domImg.naturalHeight,
                        0, 0,
                        this.domCanvas1.width, this.domCanvas1.height);
                console.log(ctx.getImageData(0, 0, 2, 2));
            });
        }
    }
    convertImage(){
        const ctx1 = this.domCanvas1.getContext("2d");
        const imageData = ctx1.getImageData(0, 0, this.domCanvas1.width, this.domCanvas1.height);
        const ctx2 = this.domCanvas2.getContext("2d");
        ctx2.putImageData(imageData, 0, 0);
    }
}
```



ペーストしてからボタンを押したら俺たちのケイジがコピーされた。

![](Pasted%20image%2020220726133812.png)


では脱線して加工してみる。

簡単にグレースケール加工してみよう。

グレースケールというのは簡単には RGB の明るさの平均値なので、そのようにする。

```javascript
    convertImage(){
        const ctx1 = this.domCanvas1.getContext("2d");
        const imageData1 = ctx1.getImageData(0, 0, this.domCanvas1.width, this.domCanvas1.height);
        const imageData2 = new ImageData(this.domCanvas1.width, this.domCanvas1.height);
        for(let i = 0; i < imageData1.data.length; i += 4){
            
            const result = (imageData1.data[i] + imageData1.data[i + 1] + imageData1.data[i + 2]) / 3;
            imageData2.data[i] = imageData2.data[i + 1] = imageData2.data[i + 2] = result;
            imageData2.data[i + 3] = 255;
        }
        const ctx2 = this.domCanvas2.getContext("2d");
        ctx2.putImageData(imageData2, 0, 0);
    }
```

そのように書いたらそのようになる。面白い。


![](Pasted%20image%2020220726135831.png)


しかし実際の印象とはやや変わってしまうようで、これを補正する係数があるようなので、これを採用する。

このような計算式のようだ。

```
0.2126 * R + 0.7152 * G + 0.0722 * B
```

緑成分が入っていると明るく感じるという特性を入れているように見える。
デジカメとかでも緑成分のセンサーが多いので、人間の感覚特性がそうなっていることを入れ込んでいる。

![](Pasted%20image%2020220726145515.png)

やってみたがほとんど違いが無い。しかし、アルゴリズムが違うのに見た目に違いが無いというのもスゴイ。
つまりそもそも人間が赤と青はほとんど見てないと言っているのである。


それでは、次はモザイク化してみようか。

これは複数のピクセルを同時に扱うのでやや考える必要がある。

ぼんやりと現物合わせで切ったりはったりして作っていくことにする。
このへんを作るのが試作品の楽しい所ではある。

```javascript
    convertImage(){
        const ctx1 = this.domCanvas1.getContext("2d");
        const imageData1 = ctx1.getImageData(0, 0, this.domCanvas1.width, this.domCanvas1.height);
        const imageData2 = new ImageData(this.domCanvas1.width, this.domCanvas1.height);
        const mzSizeX = 5;
        const mzSizeY = 5;
        for(let by = 0; by < this.domCanvas1.height / mzSizeY; by++){
            console.log(by);
            for(let bx = 0; bx < this.domCanvas1.width / mzSizeX; bx++){
                let rSum = 0;
                let gSum = 0;
                let bSum = 0;
                for(let py = 0; py < mzSizeY; py++){
                    for(let px = 0; px < mzSizeX; px++){
                        let idx = ((bx * mzSizeX) + px + (((by * mzSizeY) + py) * this.domCanvas1.width)) * 4;
                        rSum += imageData1.data[idx];
                        gSum += imageData1.data[idx + 1];
                        bSum += imageData1.data[idx + 2];
                    }
                }
                let rAvg = rSum / (mzSizeX * mzSizeY);
                let gAvg = gSum / (mzSizeX * mzSizeY);
                let bAvg = bSum / (mzSizeX * mzSizeY);
                for(let py = 0; py < mzSizeY; py++){
                    for(let px = 0; px < mzSizeX; px++){
                        let idx = ((bx * mzSizeX) + px + (((by * mzSizeY) + py) * this.domCanvas1.width)) * 4;
                        imageData2.data[idx] = rAvg;
                        imageData2.data[idx + 1] = gAvg;
                        imageData2.data[idx + 2] = bAvg;
                        imageData2.data[idx + 3] = 255;
                    }
                }
            }
        }

        const ctx2 = this.domCanvas2.getContext("2d");
        ctx2.putImageData(imageData2, 0, 0);
    }
```


ややネストが深いが、これは関係性を持っているネストなので、許容範囲な気もする。

![](Pasted%20image%2020220726163516.png)

こうなる。それっぽくモザイク処理された。

それではやや半端サイズの画像でやるとどうなるのだろうか？

![](Pasted%20image%2020220726163730.png)

この画像は、さっきよりも横が 3px だけでかい。
やるとこの 3px が切れてしまうので対応してみよう。

書いていくとややゴチャゴチャになってきたし、画像操作が  MyApp クラスの中にあってダサいので
スッキリと書き直す。

まず問題なのは、この画像の読込みの微妙な同期処理で、こいつのせいでイロイログチャ感が出てしまっているので、現代っぽく Promise で書き換える


```javascript
class MyImage{
    constructor(_d){
        this.d = _d;
        this.domCamvas = document.createElement("canvas");
        this.domImg = new Image();
        this.loaded = false;
    }
    toDomImg(){
        return new Promise((r, e)=>{
            if(this.loaded){
                return r(this.domImg);
            }
            this.domImg.addEventListener("load", ()=>{
                this.loaded = true;
                return r(this.domImg);
            });
            this.domImg.src = this.toUrl();
        });
    }
    toUrl(){
        return URL.createObjectURL(this.d);
    }
}
```









