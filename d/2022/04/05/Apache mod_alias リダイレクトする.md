---
title: Apache / mod_alias
---

[mod\_alias \- Apache HTTP サーバ バージョン 2\.4](https://httpd.apache.org/docs/2.4/mod/mod_alias.html)


リダイレクトする

`Redirect` という命令があってこいつで同一ドメイン内、異ドメインへのリダイレクトが可能

このように書く

```apache
Redirect 301 /hoge/piyo/fuga.html /foo/bar/buzz.html
Redirect 301 /hoge/piyo/fugafuga.html https://example.com/fugafuga.html
```

正規表現とか関係無く、単純に一対一で飛ばす場合はこれが楽に書ける

ディレクトリの指定は注意が必要でより長いパスが設定に含まれていた場合そちらよりもディレクトリ指定が先に採用されてしまう可能性がある。なのでそのような場合はディレクトリ指定を後ろに書く必要がある。

```apache
Redirect 301 /hoge/piyo/fuga.html /foo/bar/buzz.html
Redirect 301 /hoge/ /foo/
```


これを `/hoge/` 側を先に書くと `/hoge/piyo/fuga.html` でアクセスしても `/foo/piyo/fuga.html` に飛んで仕舞うのでディレクトリ指定はより詳細なファイル指定よりも後に書かなければならない。


