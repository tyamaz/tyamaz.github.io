---
title: Javascript_フォームを送信しているのに_submit_イベントが発生しない
aliases:
  - Javascript_form_の_submit_イベント
tags:
  - d/2008/09/30
  - n/PGM/browser/Firefox/v3.0.3
  - n/PGM/JavaScript/Ope/ブラウザ操作/イベント/submit
---


- 2008年09月30日
- Firefox 3.0.3


※注 かなり古い情報です


僕も発生すると思ってましたがフォームの submit イベントというのはフォームの送信直前に発生するものではなくフォーム中のsubmit ボタンの `onclick` の後に発生するようです。

つまりフォームの `submit` メソッドを直接呼び出しても `submit` イベントは発生しないということです。

各種ライブラリを使うとこの辺の動きをっぽく制御しなおしてくれたりする

