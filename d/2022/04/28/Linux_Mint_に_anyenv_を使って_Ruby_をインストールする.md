---
title: Linux Mint に anyenv を使って Ruby をインストールする
aliases:
  - Linux_Mint_に_anyenv_を使って_Ruby_をインストールする
tags:
  - d/2022/04/28
---


Linux Mint 20.1

[Linux Mint \- Wikipedia](https://ja.wikipedia.org/wiki/Linux_Mint) を確認すると、これは `Ubuntu 20.04 LTS` に相当するのでそのへんでも同じ風に入れられるはず。


anyenv のインストール
================================================================================

2022年なので何か変化があるかわからないのでとりあえず正攻法で入れる。

とりあえず調べるが無い

```console
$ apt search anyenv
```


公式を見る [GitHub \- anyenv/anyenv: All in one for \*\*env](https://github.com/anyenv/anyenv)

↓やってから

```console
$ git clone https://github.com/anyenv/anyenv ~/.anyenv
```

パスとか通す

```console
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bashrc
```

terminal を立ち上げ直してドン

```console
$ anyenv --version
anyenv 1.1.4-2-g2ba77e5
```

バージョンはこんな感じ

初期化操作が必要のようで

```console
$ anyenv init
# Load anyenv automatically by adding
# the following to ~/.bash_profile:

eval "$(anyenv init -)"
```


↑と同様に `.bashrc` に書いておく


terminal を立ち上げ直す

```console
ANYENV_DEFINITION_ROOT(/home/hoge/.config/anyenv/anyenv-install) doesn't exist. You can initialize it by:
> anyenv install --init
```

だそうなので実行する

```console
$ anyenv install --init
Manifest directory doesn't exist: /home/hoge/.config/anyenv/anyenv-install
Do you want to checkout https://github.com/anyenv/anyenv-install.git? [y/N]: y
Cloning https://github.com/anyenv/anyenv-install.git master to /home/hoge/.config/anyenv/anyenv-install...
Cloning into '/home/hoge/.config/anyenv/anyenv-install'...
remote: Enumerating objects: 71, done.
remote: Counting objects: 100% (14/14), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 71 (delta 4), reused 3 (delta 1), pack-reused 57
Unpacking objects: 100% (71/71), 13.13 KiB | 194.00 KiB/s, done.
Completed!
```


できた。

とりあえずインストールできるやつ一覧を見る

```console
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
  kubectlenv
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


`rbenv` がある。OK



rbenv のインストール
================================================================================

```console
$ anyenv install rbenv
```


terminal を立ち上げ直す


```console
$ rbenv --version
rbenv 1.2.0-14-gc6cc0a1
```


はい完了


Ruby のインストール
================================================================================

インストールできる Ruby の実装一覧を見る

```
$ rbenv install -l
2.6.10
2.7.6
3.0.4
3.1.2
jruby-9.3.4.0
mruby-3.0.0
rbx-5.0
truffleruby-22.1.0
truffleruby+graalvm-22.1.0

Only latest stable releases for each Ruby implementation are shown.
Use 'rbenv install --list-all / -L' to show all local versions.
```

安定版のメジャーバージョンだけ絞ってくれているらしい。うれしい。
ということで `3.1.2` を入れることにする。

```
$ rbenv install 3.1.2
```

ビルドに失敗する

```
Last 10 log lines:
The Ruby openssl extension was not compiled.
The Ruby zlib extension was not compiled.
ERROR: Ruby install aborted due to missing extensions
Try running `apt-get install -y libssl-dev zlib1g-dev` to fetch missing dependencies.
```

だとよ

```console
$ sudo apt install -y libssl-dev zlib1g-dev
```


再び

```console
$ rbenv install 3.1.2
```

OK

```
$ rbenv versions
  3.1.2
```


こいつを使う Ruby に指定する

```console
$ rbenv global 3.1.2
```

OK

```console
$ ruby --version
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [x86_64-linux]
```


```console
$ gem --version
3.3.7
```




















