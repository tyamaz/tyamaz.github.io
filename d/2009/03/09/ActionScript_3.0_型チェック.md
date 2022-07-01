---
title: ActionScript 3.0 型チェック
aliases:
  - ActionScript_3.0_型チェック
tags:
  - d/2009/03/09
  - n/PGM/ActionScript/3.0
---

- 2009年03月09日
- ActionScript3.0


スーパークラスまで比較
================================================================================
スーパークラスまで一致を遡るチェックには `is` 演算子を使う

```actionscript
var hoge:int = 12; 
trace (hoge is int); //=> true
```

由来クラスの一致をチェック（スーパークラスまで比較しないやつ）
================================================================================
Javascript と同じで `constructor` プロパティが使えます
```actionscript
var d:Date = new Date();
trace(d.constructor === Date); //=>true;
trace(d.constructor === Object); //=>false;
```

※これ使うとコンパイラから警告が出る・・・アドビはメタプログラム的な使い方非推奨ってことかな・・・

クラスそのものを知る
================================================================================
ま、適当に

```actionscript
var d:Date = new Date();
trace(d.constructor.toString()); //=> [class Date]
```

