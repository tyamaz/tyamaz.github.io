---
title: Google_検索パラメータ
aliases:
  - Google_検索パラメータ
tags:
  - d/2009/02/09
  - n/PGM/etc/Google
---

- 2009年02月09日
- Google 2009年02月09日頃バージョン

検索窓のコードを眺める
================================================================================
本質じゃないコードを取っ払うと↓

```html
<form method=get action="http://www.google.co.jp/search">
    <input type=text name=q size=31 maxlength=255 value="">
    <input type=hidden name=ie value=Shift_JIS>
    <input type=hidden name=oe value=Shift_JIS>
    <input type=hidden name=hl value="ja">
    <input type=submit name=btnG value="Google 検索">
</form>
```

ってことで

```
http://www.google.co.jp/search?ie=UTF-8&oe=UTF-8&hl=ja&q=hoge
```

こんなんで大丈夫だろう

一応2バイトも試しておく

```
http://www.google.co.jp/search?ie=UTF-8&oe=UTF-8&hl=ja&q=%e3%81%bb%e3%81%92
```
