---
title: anyenv
aliases: [anyenv]
tags:
  - d/2021/12/10
  - n/PGM/gg/Apple_Macbook_Air_M1_2020/anyenv
---

Apple Macbook Air M1 2020 環境での話

2021-12-10 現在の方法

公式に従う
https://github.com/anyenv/anyenv



```console
$ brew install anyenv
$ anyenv init
```


見た感じ `arm64` 版が入ったっぽい

```console
# Load anyenv automatically by adding
# the following to ~/.zshrc:
eval "$(anyenv init -)"
```

らしいので、`~/.zshrc` に書けよという話

そもそも `.zshrc` が無いので作って書き込んでおく

OK

シェルを再起動する。

そうするとこのようなメッセージが出る

```console
ANYENV_DEFINITION_ROOT(/Users/hogehoge/.config/anyenv/anyenv-install) doesn't exist. You can initialize it by:
> anyenv install --init
```

なのでこのコマンドを打つ

```
$ anyenv install --init
```

結果こうらしい

```
Manifest directory doesn't exist: /Users/hogehoge/.config/anyenv/anyenv-install
Do you want to checkout https://github.com/anyenv/anyenv-install.git? [y/N]: y
Cloning https://github.com/anyenv/anyenv-install.git master to /Users/ya/.config/anyenv/anyenv-install...
Cloning into '/Users/hogehoge/.config/anyenv/anyenv-install'...
remote: Enumerating objects: 62, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 62 (delta 1), reused 1 (delta 0), pack-reused 57
Receiving objects: 100% (62/62), 10.52 KiB | 3.51 MiB/s, done.
Resolving deltas: 100% (8/8), done.

Completed!
```

ま、なんというかインストールできる env リストみたいなもんなんだろうか

とりあえず確認はしてみる そのようだ

ということでリストを確認する

```
$ anyenv install -l
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

ということで `nodenv` をインストールする

```console
$ anyenv install nodenv
```

最後にシェルを再起動しろみたいな指示が出るので再起動する

`nodenv` を確認する

```console
$ nodenv --version
nodenv 1.4.0+3.631d0b6
```

インストールできる `node` を確認する


```console
$ nodenv install -l
0.1.14
0.1.15
0.1.16
0.1.17
0.1.18
0.1.19
0.1.20
0.1.21
0.1.22
0.1.23
0.1.24
0.1.25
0.1.26
0.1.27
0.1.28
0.1.29
0.1.30
0.1.31
0.1.32
0.1.33
0.1.90
0.1.91
0.1.92
0.1.93
0.1.94
0.1.95
0.1.96
0.1.97
0.1.98
0.1.99
0.1.100
0.1.101
0.1.102
0.1.103
0.1.104
0.2.0
0.2.1
0.2.2
0.2.3
0.2.4
0.2.5
0.2.6
0.3.0
0.3.1
0.3.2
0.3.3
0.3.4
0.3.5
0.3.6
0.3.7
0.3.8
0.4.0
0.4.1
0.4.2
0.4.3
0.4.4
0.4.5
0.4.6
0.4.7
0.4.8
0.4.9
0.4.10
0.4.11
0.4.12
0.5.0
0.5.1
0.5.2
0.5.3
0.5.4
0.5.5
0.5.6
0.5.7
0.5.8
0.5.9
0.5.10
0.6.0
0.6.1
0.6.2
0.6.3
0.6.4
0.6.5
0.6.6
0.6.7
0.6.8
0.6.9
0.6.10
0.6.11
0.6.12
0.6.13
0.6.14
0.6.15
0.6.16
0.6.17
0.6.18
0.6.19
0.6.20
0.6.21
0.7.0
0.7.1
0.7.2
0.7.3
0.7.4
0.7.5
0.7.6
0.7.7
0.7.8
0.7.9
0.7.10
0.7.11
0.7.12
0.8.0
0.8.1
0.8.2
0.8.3
0.8.4
0.8.5
0.8.6
0.8.7
0.8.8
0.8.9
0.8.10
0.8.11
0.8.12
0.8.13
0.8.14
0.8.15
0.8.16
0.8.17
0.8.18
0.8.19
0.8.20
0.8.21
0.8.22
0.8.23
0.8.24
0.8.25
0.8.26
0.8.27
0.8.28
0.9.0
0.9.1
0.9.2
0.9.3
0.9.4
0.9.5
0.9.6
0.9.7
0.9.8
0.9.9
0.9.10
0.9.11
0.9.12
0.10.0
0.10-dev
0.10-next
0.10.1
0.10.2
0.10.3
0.10.4
0.10.5
0.10.6
0.10.7
0.10.8
0.10.9
0.10.10
0.10.11
0.10.12
0.10.13
0.10.14
0.10.15
0.10.16
0.10.17
0.10.18
0.10.19
0.10.20
0.10.21
0.10.22
0.10.23
0.10.24
0.10.25
0.10.26
0.10.27
0.10.28
0.10.29
0.10.30
0.10.31
0.10.32
0.10.33
0.10.34
0.10.35
0.10.36
0.10.37
0.10.38
0.10.39
0.10.40
0.10.41
0.10.42
0.10.43
0.10.44
0.10.45
0.10.46
0.10.47
0.10.48
0.11.0
0.11.1
0.11.2
0.11.3
0.11.4
0.11.5
0.11.6
0.11.7
0.11.8
0.11.9
0.11.10
0.11.11
0.11.12
0.11.13
0.11.14
0.11.15
0.11.16
0.12.0
0.12-dev
0.12-next
0.12.1
0.12.2
0.12.3
0.12.4
0.12.5
0.12.6
0.12.7
0.12.8
0.12.9
0.12.10
0.12.11
0.12.12
0.12.13
0.12.14
0.12.15
0.12.16
0.12.17
0.12.18
4.0.0
4.x-dev
4.x-next
4.1.0
4.1.1
4.1.2
4.2.0
4.2.1
4.2.2
4.2.3
4.2.4
4.2.5
4.2.6
4.3.0
4.3.1
4.3.2
4.4.0
4.4.1
4.4.2
4.4.3
4.4.4
4.4.5
4.4.6
4.4.7
4.5.0
4.6.0
4.6.1
4.6.2
4.7.0
4.7.1
4.7.2
4.7.3
4.8.0
4.8.1
4.8.2
4.8.3
4.8.4
4.8.5
4.8.6
4.8.7
4.9.0
4.9.1
5.0.0
5.x-next
5.1.0
5.1.1
5.2.0
5.3.0
5.4.0
5.4.1
5.5.0
5.6.0
5.7.0
5.7.1
5.8.0
5.9.0
5.9.1
5.10.0
5.10.1
5.11.0
5.11.1
5.12.0
6.0.0
6.x-dev
6.x-next
6.1.0
6.2.0
6.2.1
6.2.2
6.3.0
6.3.1
6.4.0
6.5.0
6.6.0
6.7.0
6.8.0
6.8.1
6.9.0
6.9.1
6.9.2
6.9.3
6.9.4
6.9.5
6.10.0
6.10.1
6.10.2
6.10.3
6.11.0
6.11.1
6.11.2
6.11.3
6.11.4
6.11.5
6.12.0
6.12.1
6.12.2
6.12.3
6.13.0
6.13.1
6.14.0
6.14.1
6.14.2
6.14.3
6.14.4
6.15.0
6.15.1
6.16.0
6.17.0
6.17.1
7.0.0
7.x-dev
7.x-next
7.1.0
7.2.0
7.2.1
7.3.0
7.4.0
7.5.0
7.6.0
7.7.0
7.7.1
7.7.2
7.7.3
7.7.4
7.8.0
7.9.0
7.10.0
7.10.1
8.0.0
8.x-dev
8.x-next
8.1.0
8.1.1
8.1.2
8.1.3
8.1.4
8.2.0
8.2.1
8.3.0
8.4.0
8.5.0
8.6.0
8.7.0
8.8.0
8.8.1
8.9.0
8.9.1
8.9.2
8.9.3
8.9.4
8.10.0
8.11.0
8.11.1
8.11.2
8.11.3
8.11.4
8.12.0
8.13.0
8.14.0
8.14.1
8.15.0
8.15.1
8.16.0
8.16.1
8.16.2
8.17.0
9.0.0
9.x-dev
9.x-next
9.1.0
9.2.0
9.2.1
9.3.0
9.4.0
9.5.0
9.6.0
9.6.1
9.7.0
9.7.1
9.8.0
9.9.0
9.10.0
9.10.1
9.11.0
9.11.1
9.11.2
10.0.0
10.x-dev
10.x-next
10.1.0
10.2.0
10.2.1
10.3.0
10.4.0
10.4.1
10.5.0
10.6.0
10.7.0
10.8.0
10.9.0
10.10.0
10.11.0
10.12.0
10.13.0
10.14.0
10.14.1
10.14.2
10.15.0
10.15.1
10.15.2
10.15.3
10.16.0
10.16.1
10.16.2
10.16.3
10.17.0
10.18.0
10.18.1
10.19.0
10.20.0
10.20.1
10.21.0
10.22.0
10.22.1
10.23.0
10.23.1
10.23.2
10.23.3
10.24.0
10.24.1
11.0.0
11.x-dev
11.x-next
11.1.0
11.2.0
11.3.0
11.4.0
11.5.0
11.6.0
11.7.0
11.8.0
11.9.0
11.10.0
11.10.1
11.11.0
11.12.0
11.13.0
11.14.0
11.15.0
12.0.0
12.x-dev
12.x-next
12.1.0
12.2.0
12.3.0
12.3.1
12.4.0
12.5.0
12.6.0
12.7.0
12.8.0
12.8.1
12.9.0
12.9.1
12.10.0
12.11.0
12.11.1
12.12.0
12.13.0
12.13.1
12.14.0
12.14.1
12.15.0
12.16.0
12.16.1
12.16.2
12.16.3
12.17.0
12.18.0
12.18.1
12.18.2
12.18.3
12.18.4
12.19.0
12.19.1
12.20.0
12.20.1
12.20.2
12.21.0
12.22.0
12.22.1
12.22.2
12.22.3
12.22.4
12.22.5
12.22.6
12.22.7
13.0.0
13.x-dev
13.x-next
13.0.1
13.1.0
13.2.0
13.3.0
13.4.0
13.5.0
13.6.0
13.7.0
13.8.0
13.9.0
13.10.0
13.10.1
13.11.0
13.12.0
13.13.0
13.14.0
14.0.0
14.x-dev
14.x-next
14.1.0
14.2.0
14.3.0
14.4.0
14.5.0
14.6.0
14.7.0
14.8.0
14.9.0
14.10.0
14.10.1
14.11.0
14.12.0
14.13.0
14.13.1
14.14.0
14.15.0
14.15.1
14.15.2
14.15.3
14.15.4
14.15.5
14.16.0
14.16.1
14.17.0
14.17.1
14.17.2
14.17.3
14.17.4
14.17.5
14.17.6
14.18.0
14.18.1
14.18.2
15.0.0
15.0.1
15.1.0
15.2.0
15.2.1
15.3.0
15.4.0
15.5.0
15.5.1
15.6.0
15.7.0
15.8.0
15.9.0
15.10.0
15.11.0
15.12.0
15.13.0
15.14.0
16.0.0
16.1.0
16.2.0
16.3.0
16.4.0
16.4.1
16.4.2
16.5.0
16.6.0
16.6.1
16.6.2
16.7.0
16.8.0
16.9.0
16.9.1
16.10.0
16.11.0
16.11.1
16.12.0
16.13.0
16.13.1
17.0.0
17.0.1
17.1.0
17.2.0
chakracore-dev
chakracore-nightly
chakracore-8.1.2
chakracore-8.1.4
chakracore-8.2.1
chakracore-8.3.0
chakracore-8.4.0
chakracore-8.6.0
chakracore-8.9.4
chakracore-8.10.0
chakracore-8.11.1
chakracore-10.0.0
chakracore-10.1.0
chakracore-10.6.0
chakracore-10.13.0
graal+ce-1.0.0-rc1
graal+ce-1.0.0-rc10
graal+ce-1.0.0-rc11
graal+ce-1.0.0-rc12
graal+ce-1.0.0-rc13
graal+ce-1.0.0-rc14
graal+ce-1.0.0-rc15
graal+ce-1.0.0-rc16
graal+ce-1.0.0-rc2
graal+ce-1.0.0-rc3
graal+ce-1.0.0-rc4
graal+ce-1.0.0-rc5
graal+ce-1.0.0-rc6
graal+ce-1.0.0-rc7
graal+ce-1.0.0-rc8
graal+ce-1.0.0-rc9
graal+ce-19.0.0
graal+ce-19.0.2
graal+ce-19.1.0
graal+ce-19.1.1
graal+ce-19.2.0
graal+ce-19.2.0.1
graal+ce-19.2.0-dev-b01
graal+ce-19.2.1
graal+ce_java11-19.3.0
graal+ce_java11-19.3.0.2
graal+ce_java8-19.3.0
graal+ce_java8-19.3.0.2
graal+ce_java11-19.3.1
graal+ce_java8-19.3.1
graal+ce_java11-20.0.0
graal+ce_java8-20.0.0
iojs-0.12.0-dev
iojs-1.0.0
iojs-1.x-dev
iojs-1.0.1
iojs-1.0.2
iojs-1.0.3
iojs-1.0.4
iojs-1.1.0
iojs-1.2.0
iojs-1.3.0
iojs-1.4.1
iojs-1.4.2
iojs-1.4.3
iojs-1.5.0
iojs-1.5.1
iojs-1.6.0
iojs-1.6.1
iojs-1.6.2
iojs-1.6.3
iojs-1.6.4
iojs-1.7.1
iojs-1.8.1
iojs-1.8.2
iojs-1.8.3
iojs-1.8.4
iojs-2.0.0
iojs-2.0.1
iojs-2.0.2
iojs-2.1.0
iojs-2.2.0
iojs-2.2.1
iojs-2.3.0
iojs-2.3.1
iojs-2.3.2
iojs-2.3.3
iojs-2.3.4
iojs-2.4.0
iojs-2.5.0
iojs-3.0.0
iojs-3.1.0
iojs-3.2.0
iojs-3.3.0
iojs-3.3.1
nightly
node-dev
rc
v8-canary

```


`17.2.0` が最新っぽいので、こいつを入れることにする

```console
$ nodenv install 17.2.0
Downloading node-v17.2.0-darwin-arm64.tar.gz...
-> https://nodejs.org/dist/v17.2.0/node-v17.2.0-darwin-arm64.tar.gz
Installing node-v17.2.0-darwin-arm64...
Installed node-v17.2.0-darwin-arm64 to /Users/hogehoge/.anyenv/envs/nodenv/versions/17.2.0
```

`arm64` 版が入ったようだ

```console
$ anyenv versions
nodenv:
  17.2.0
```

こいつをとりあえずの標準とする

```console
$ nodenv global 17.2.0
```

```console
$ nodenv global 17.2.0
$ node --version
v17.2.0
```






