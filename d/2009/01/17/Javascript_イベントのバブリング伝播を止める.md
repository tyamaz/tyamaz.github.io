---
title: Javascript_イベントのバブリング伝播を止める
aliases:
  - Javascript_イベントのバブリング伝播を止める
tags:
  - d/2009/01/17
  - n/PGM/JavaScript/Ope/ブラウザ操作/イベント
---

- 2009年01月17日
- Firefox3.0.2

バブリング伝播を止める
================================================================================
イベント伝播で起動されたメソッド内で、止める処理を書くことによって伝播が上に伝わることを阻止できる。


Firefox版

```javascript
//eは第一引数で渡されるeventのオブジェクト
e.stopPropagation();
```

IE版

```javascript
window.event.cancelBubble = true;
```

