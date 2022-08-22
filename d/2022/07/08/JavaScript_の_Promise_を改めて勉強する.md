---
title: JavaScript の Promise を改めて勉強する
aliases:
  - JavaScript_の_Promise_を改めて勉強する
tags:
  - d/2022/07/08
---

`await` を使いすぎて理解がぼやっとしてきたので改めて勉強してみる

素の Promise
================================================================================
```javascript
const a = "hello";
const b = "hello";

const hoge = new Promise(function(resolve, reject){
    console.log('start promise');
    if(a == b){
        console.log('aaa');
        resolve();
    }else{
        console.log('bbb');
        reject();
    }
    console.log('finish promise');
});
```

`Promise`  を素で `new` している。理由は無い。素で作るとどうなるのかを知る。

このコードを実行すると、Promise のコンストラクタに渡している関数は実行してないのに、勝手に実行される。


