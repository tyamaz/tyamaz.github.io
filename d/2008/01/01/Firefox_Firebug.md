---
title: Firefox Firebug
aliases:
  - Firefox / Firebug
  - Firebug
tags:
  - d/2008/01/01
  - n/PGM/browser/Firefox/Firebug/v1.05
---


id指定でDOMを特定
================================================================================

```javascript
$("hoge");
```

XPath指定でDOMを特定
================================================================================

```javascript
$x("//hoge[@piyo]");
```


ブレークポイントで止まらない
================================================================================
バージョン1.05

止まらなかったので・・・なんでかなと思って。

Firefoxのウィンドウを複数開いて、両方でブレークポイントを指定してとめると片方でとめているときにはもう一方のFirebugでブレークポイントで止まらなくなります。

