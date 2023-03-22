---
title: Cygwin RubyGemsをインストールする
aliases:
  - Cygwin_RubyGemsをインストールする
tags:
  - d/2009/09/29
  - n/PGM/Cygwin
  - n/PGM/Ruby/RubyGems/v1.3.5
---

- 2009年09月29日
- rubygems 1.3.5

ダウンロード
================================================================================
ここらへんからダウンロード
[RubyForge: RubyGems: ファイルリスト](http://rubyforge.org/frs/?group_id=126)

今回はバージョン 1.3.5 の `rubygems-1.3.5.tgz` これを使った。


場所はこの辺 `D:\setup\rubygems-1.3.5.tgz`


インストール
================================================================================
まず解凍

```
$ tar xvzf /cygdrive/d/setup/rubygems-1.3.5.tgz
```

カレントディレクトリ、今ならユーザーのホームディレクトリにファイルが解凍される。

移動して

```
$ cd rubygems-1.3.5.tgz
```

インストールのスクリプトらしきやつを叩く

```
$ ruby setup.rb
```

こんなんが出たのでたぶんインストールできたのでしょう

```
RubyGems installed the following executables:
        /usr/bin/gem
````


確認
================================================================================

更新のコマンドを打ってみる

```
$ gem update
$ gem update --system
```


動作が確認できたのでOK

