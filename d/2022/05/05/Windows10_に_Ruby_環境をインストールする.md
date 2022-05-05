---
title: Windows10_に_Ruby_環境をインストールする
aliases:
  - Windows10_に_Ruby_環境をインストールする
tags:
  - d/2022/05/05
---

Windows はそもそもそんなにガチンコで仕事で使う環境では無いので単にインストーラーで入れるだけ

[Downloads](https://rubyinstaller.org/downloads/) ここから `Ruby+Devkit 3.1.X (x64)` みたいなやつをダウンロードする。

今回は `Ruby+Devkit 3.1.2-1 (x64)` を使った


インストーラーダブルクリックするだけ。

```console
> ruby --version  
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [x64-mingw-ucrt]
```

OK

```console
> gem --version  
3.3.7
```


`bundler` を突っ込む

```console
> gem install bundler  
Fetching bundler-2.3.13.gem  
Successfully installed bundler-2.3.13  
Parsing documentation for bundler-2.3.13  
Installing ri documentation for bundler-2.3.13  
Done installing documentation for bundler after 0 seconds  
1 gem installed  

> bundle --version  
Bundler version 2.3.13
```

OK

