---
title: anyenv に anyenv-update plugin を導入する
aliases:
  - anyenv_に_plugin_を導入する
tags:
  - d/2022/07/13
  - n/PGM/Linux/anyenv
---

Linux Mint 20.1 環境

現状バージョン確認

```console
$ anyenv --version
anyenv 1.1.4-2-g2ba77e5
```



インストール

```console
$ mkdir -p $(anyenv root)/plugins
$ git clone https://github.com/znz/anyenv-update.git $(anyenv root)/plugins/anyenv-update
```


実行

```console
$ anyenv update
$ anyenv update
Updating 'anyenv'...
 |  From https://github.com/anyenv/anyenv
 |  2ba77e5..95a0419  master     -> origin/master
Updating 'anyenv/anyenv-update'...
Updating 'nodenv'...
 |  From https://github.com/nodenv/nodenv
 |  631d0b6..acf64b3  master     -> origin/master
 |  * [new branch]      depfu/update/npm/bats-1.7.0 -> origin/depfu/update/npm/bats-1.7.0
Updating 'nodenv/node-build'...
 |  From https://github.com/nodenv/node-build
 |  066b2c7e..53ea13b5  master     -> origin/master
 |  * [new branch]        depfu/update/npm/bats-1.7.0 -> origin/depfu/update/npm/bats-1.7.0
 |  * [new tag]           v4.9.85    -> v4.9.85
 |  * [new tag]           v4.9.80    -> v4.9.80
 |  * [new tag]           v4.9.81    -> v4.9.81
 |  * [new tag]           v4.9.82    -> v4.9.82
 |  * [new tag]           v4.9.83    -> v4.9.83
 |  * [new tag]           v4.9.84    -> v4.9.84
Updating 'nodenv/nodenv-vars'...
Updating 'rbenv'...
Updating 'rbenv/ruby-build'...
 |  From https://github.com/rbenv/ruby-build
 |  deb3dd0..c0d6e22  master     -> origin/master
 |  * [new tag]         v20220610  -> v20220610
 |  * [new tag]         v20220630  -> v20220630
 |  * [new tag]         v20220710  -> v20220710
Updating 'anyenv manifest directory'...
```


微妙に上がっていることがわかる

```console
$ anyenv --version
anyenv 1.1.4-5-g95a0419
```

