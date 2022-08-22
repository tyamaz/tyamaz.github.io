---
title: Linux Mint に SQLite3 をインストールする
tags:
  - d/2022/04/29
  - n/PGM/Linux/d/Linux_Mint/20.1
  - n/PGM/DB/SQLite3
---

2022-04-29 環境 Linux Mint 20.1

調べる。

```console
$ apt search sqlite
...
p   sqlite                          - command line interface for SQLite 2
p   sqlite3                         - Command line interface for SQLite 3
...
```


インストール

```console
$ sudo apt install sqlite3
```


完

```console
$ sqlite3 --version
3.31.1 2020-01-27 19:55:54 3bfa9cc97da10598521b342961df8f5f68c7388fa117345eeb516eaa837balt1
```

