---
title: Apache mod_rewrite 異なるサイトへリダイレクトする
aliases:
  - Apache mod_rewrite 異なるサイトへリダイレクトする
tags:
  - PGM/Apache/mod_rewrite
---


異なるサイトへリダイレクトする
サイトの引っ越しがあったような場合にやるやつ

```apache
RewriteRule ^hoge/piyo/fuga\.html http://example.com/hogehoge/piyopiyo/fugafuga.html [R=301,L]
```

この元のファイルのパスの先頭の `hoge` はドメインのルート部分に存在している `hoge` ディレクトリを指している。


