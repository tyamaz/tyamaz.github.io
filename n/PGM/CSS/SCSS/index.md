---
title: SCSS
aliases:
  - SCSS
tags:
  - CSS
---


SCSS 事情
================================================================================
SCSS はイロイロ経緯があるっぽい。

まずそもそもとして `SCSS` は `Sass` というCSS生成言語の拡張仕様として登場している。なので環境として `Sass` の影響を受ける。

`Sass` というのは 「Syntactically Awesome StyleSheet」 の略らしいが、この書き方で書いてる人間なんかほとんど存在しないほどいけてない書き方だった。そこで似たような CSS生成環境であった `LESS` のアイデアを丸パクリして登場したのが `SCSS` ということになっている。

そしてこの `Sass` にも経緯があって、実装がいくつか存在し、全部が現在進行でバリバリ開発中ならよいのだが、それが開発が止まってしまっているモノもあり・・・

- Ruby Sass
  - オリジナルの Ruby 実装。動作が低速だということで開発終了している、が Ruby 環境では未だに使われていることが多い。つまりこいつは仕様が古い。
- LibSass
  - C++ 実装。他の環境が読み込んで使う前提
- Dart Sass
  - Dart 実装。現在進行形の公式の推奨環境。動作が速い。
- jSass
  - Java 実装。Ruby にはそもそも JRuby という Java 実装があるのであえて作る理由があんのかとも思う。

というような感じなので、どの環境かによって微妙に仕様が変わってくる。
