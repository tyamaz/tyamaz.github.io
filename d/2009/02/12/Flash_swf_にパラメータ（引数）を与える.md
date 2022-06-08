---
title: Flash swf にパラメータ（引数）を与える
aliases:
  - Flash_swf_にパラメータ（引数）を与える
tags:
  - d/2009/02/12
  - n/PGM/Flash
---


- 2009年02月12日
- Flash Player 9
- Flash CS3
- ActionScript3.0


FlashVars 属性を使って渡す
================================================================================
`object` タグなら これを埋め込む

```xml
<param name="FlashVars" value="hoge=1&piyo=2">
```

`embed` タグなら、属性追加

```xml
<embed src="hoge.swf" FlashVars="hoge=1&piyo=2" />
```

こんな風


プログラム側から取得する
================================================================================
プログラム側からは

```actionscript
var flashVars:Object = this.loaderInfo.parameters;
var hoge:String = flashVars['hoge'];
```

こんな風に取得できる。

ってドイツもこいつも書くもんだからその通りにやって

```
オブジェクト参照のプロパティまたはメソッドにアクセスすることはできません。
```

なんてエラーメッセージがでて途方にくれましたよ。

この書き方には制限があって、どのインスタンスからでも使えるわけじゃなくて、`loaderInfo` から引数を引き出せるのはドキュメントクラスのみです。

ac3 やってるほとんどは Flex 派のプログラマで Flash での作例が少ないよ！
