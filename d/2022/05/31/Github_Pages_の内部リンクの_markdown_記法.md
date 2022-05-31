---
title: Github Pages の内部リンクの markdown 記法
aliases:
  - Github_Pages_の_内部リンクの_markdown_記法
tags:
  - d/2022/05/31
  - n/PGM/Markdown
  - n/PGM/Github_Pages
  - n/PGM/Jekyll
---




Github Pages の Markdown で記述する内部リンクの書き方。

結論として、Github Pages の内部リンクは全て相対パスで書く必要がある

このように記述する。

```markdown
[hoge](a/b/c/hoge.md)
```

こうすると `https://example.github.io/aaaa/a/b/c/hoge.html` へのリンクに変換される。


このようにスラッシュから書けばドメインのルートからのリンクとして扱ってくれそうな気がする。

```markdown
[hoge](/a/b/c/hoge.md)
```

しかしこれは正しい内部リンクとして扱ってもらえず `hoge.html` へ変換してくれない。
`http` からフルで書いても変換してくれない。

ページのディレクトリ移動が非常に不便である。







