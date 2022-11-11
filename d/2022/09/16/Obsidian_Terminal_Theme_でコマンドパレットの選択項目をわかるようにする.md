---
title: Obsidian Terminal Theme でコマンドパレットの選択項目をわかるようにする
aliases:
  - Obsidian_Terminal_Theme_でコマンドパレットの選択項目をわかるようにする
tags:
  - d/2022/09/16
---

この `Terminal` というテーマはよいのだが、コマンドパレットから操作する際にその選択対象がわからない(基本的に一番上)のが問題だった。

それを解消する。

スタイルに↓を加えればよい

```css
/* for Theme Terminal */
.suggestion-item.is-selected{
    font-weight: bold;
    background-color: var(--the-color);
    color: var(--the-background-color);
}
```

これでハイライトされる。


