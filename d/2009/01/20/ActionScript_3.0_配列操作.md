---
title: ActionScript 3.0 配列操作
aliases:
  - ActionScript_3.0_配列操作
tags:
  - d/2009/01/20
  - n/PGM/ActionScript/v3.0/Ope 
---



ECMAから来てるから [Javascript/配列操作](Javascript/配列操作) と基本的に似ていると・・・思う。

- 2009年01月20日
- ActionScript3.0

特徴
================================================================================
古いJava的な Array で要素の型指定をサポートしていない。つまりギチギチのJava式で書くと取り出すたびにキャストが発生してウザイ。

検索系
================================================================================
指定したindexで取得する
--------------------------------------------------------------------------------
```actionscript
var hoge:Array = [1,2,3];
hoge[1]; //=> 2
```

指定したオブジェクトの存在をチェックする
--------------------------------------------------------------------------------
```actionscript
var hoge1:Hoge = new Hoge();
var hoge2:Hoge = new Hoge();
var hoge3:Hoge = new Hoge();
var hoge4:Hoge = new Hoge();

var hogeA:Array = [hoge1,hoge2,hoge3];
hogeA.indexOf(hoge2); //=> 1
hogeA.indexOf(hoge4); //=> -1
```

存在しないなら-1が返る


指定したオブジェクトの位置を知る
--------------------------------------------------------------------------------
```actionscript
var a:Hoge = new Hoge();
var b:Hoge = new Hoge();
var c:Hoge = new Hoge();
var abc:Array = [a,c,b];
abc.indexOf(b); //=> 1
```


作成系
================================================================================
配列の要素をコピーした新しい配列を作る（一部）
--------------------------------------------------------------------------------
```actionscript
var hoge:Array = ["a", "b", "c", "d", "e", "f"];
var piyo:Array = hoge.slice(1, 3);
```

配列の一部を切り出して、新たな配列を作るには `slice` メソッドを使う。
オンラインマニュアルには

```actionscript
function slice(startIndex:int = 0, endIndex:int = 16777215):Array
```

のように定義されているが、第二引数は明らかに終わりの `index` ではない。

↑の結果は

```
 a,b,c,d,e,f
 b,c
```

start と end ならば b,c,d になるはず。さらに

`endIndex` が負の数値の場合、終点は配列の末尾から開始します。
つまり、`-1` が最後のエレメントです。 

なんて書かれているが、`(1,-1)` の結果は `a,b,c,d,e` だ

第一引数 index 以上で第二引数 index 未満の index の値をコピーする挙動のようだ・・・

