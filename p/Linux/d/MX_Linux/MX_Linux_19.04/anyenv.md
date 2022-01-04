---
title: MX Linux 19.04 / anyenv
aliases:
  - MX Linux 19.04 / anyenv
  - anyenv
---

*mokuji
[:toc]

Install 2021-12-13編
================================================================================

#d/2021/12/13

公式に従う

```console
$ git clone https://github.com/anyenv/anyenv ~/.anyenv
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bashrc
```

Terminal 再起動

```
$ anyenv install --init
```


インストールする一覧を見る

```
$ anyenv install --l
  Renv
  crenv
  denv
  erlenv
  exenv
  goenv
  hsenv
  jenv
  jlenv
  luaenv
  nodenv
  phpenv
  plenv
  pyenv
  rbenv
  sbtenv
  scalaenv
  swiftenv
  tfenv
```

今回は Ruby をインストールしたいので `rbenv` を使う

```console
$ anyenv install rbenv
```

```console
$ echo 'eval "$(anyenv init -)"' >> ~/.bashrc
```

Terminal 再起動

```console
$ rbenv --version
rbenv 1.2.0-6-g304cb7b
```

OK

```console
$ rbenv install -l
2.6.9
2.7.5
3.0.3
jruby-9.3.2.0
mruby-3.0.0
rbx-5.0
truffleruby-21.3.0
truffleruby+graalvm-21.3.0
```

`3.0.3` を入れる


```console
$ rbenv install 3.0.3
```

なんか失敗した、`libssl-dev` が無いとか古いとか、そういう話らしい。

入れる

```
$ sudo apt install libssl-dev
```

再び

```console
$ rbenv install 3.0.3
```


OK

```console
$ rbenv global 3.0.3
$ ruby --version
ruby 3.0.3p157 (2021-11-24 revision 3fb7d2cadc) [x86_64-linux]
$ gem --version
3.2.32
```

OK















