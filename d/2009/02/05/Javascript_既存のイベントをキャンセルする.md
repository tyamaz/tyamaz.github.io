---
title: Javascript 既存のイベントをキャンセルする
aliases:
  - Javascript_既存のイベントをキャンセルする
tags:
  - d/2009/02/05
  - n/PGM/JavaScript/Ope/ブラウザ操作/イベント
---

- 2009年02月05日


既存のイベントをキャンセルする
================================================================================
IE

```javascript
event.returnValue = false;
```

Firefoxとか

```javascript
event.preventDefault();
```

