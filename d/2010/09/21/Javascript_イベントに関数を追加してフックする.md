---
title: Javascript イベントに関数を追加してフックする
aliases:
  - Javascript_イベントに関数を追加してフックする
tags:
  - d/2010/09/21
  - n/PGM/JavaScript/Ope/ブラウザ操作/イベント
---

- 2010-09-21

クロスで
================================================================================

```javascript
function hoge(){
    alert("hoge");
}
if(window.addEventListener){
    //モダン
    window.addEventListener("load", hoge, false);
}else if(window.attachEvent){
    //遺物
    window.attachEvent("onload", hoge);
}
```
