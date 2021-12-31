---
title: IE8 の IE7 互換モード
aliases:
  - IE8 の IE7 互換モード
tags:
  - old
  - ブラウザ
---

メンテしてるときに嵌ったのでメモ

`HTML` のヘッダーに

```html
<meta http-equiv="X-UA-Compatible" content="IE=emulateIE7" />
```

をつけると `IE8` で `IE7` の互換モード表示をするモードになる。
こいつをやると `iframe` の枠線を `JavaScript` で制御できなくなる。

`innerHTML` で生成すれば属性の値のセットはできます。

`createElement` で作った `iframe` と `innerHTML` で作った `iframe` は微妙に挙動が違う・・・。メソッドがあったりとかなかったりとか


