---
title: ActionScript3.0 日時操作
aliases:
  - ActionScript3.0_日時操作
tags:
  - d/2009/02/13
  - n/PGM/ActionScript/v3.0
---


- 2009年02月13日
- ActionScript3.0

指定日時のDateインスタンスを作る
================================================================================
基本
--------------------------------------------------------------------------------
年月日時分秒と並べる

```actionscript
new Date(1999, 12, 3, 15, 23, 47);
```

入力パラメータを一部文字列で指定してみる
--------------------------------------------------------------------------------

```actionscript
new Date(1999, "12", 3, 15, 23, 47);
```

できる！


月末を求める
================================================================================
イロイロ試してみると、月末というのは ActionScript の `Date` クラスではその月のゼロ日目という扱いらしい・・なんか変だけど。

この考えでいくと月末前日は -1 日ということになっている。
Ruby では -1 日が月末ということになっているが、まぁいいか

```actionscript
new Date(2009, 2, 0);
```

これが こうなる。

```
Sat Feb 28 00:00:00 GMT+0900 2009
```


月最終時刻を求める
================================================================================
月末もわかったので月の最終時刻を求めてみる

つうことで


```actionscript
new Date(2009, 2, 0, 23, 59, 59);
```

こうなる。



参考サイト
================================================================================
- [Date - ActionScript 3.0 コンポーネントリファレンスガイド](http://livedocs.adobe.com/flash/9.0_jp/ActionScriptLangRefV3/Date.html#propertySummary)

