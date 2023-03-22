---
title: Symfony1.2 開発手順
aliases:
  - Symfony1.2_開発手順
tags:
  - d/2023/02/24
  - n/PGM/PHP/Symfony/v1.2
---

- 2009年01月06日
- symfony1.2

流れ
================================================================================
環境構築
--------------------------------------------------------------------------------


プロジェクトの名前決め
--------------------------------------------------------------------------------
決めます

プロジェクト作成
--------------------------------------------------------------------------------
symfonyではプロジェクトという大きな塊で管理します。

アプリケーション作成
--------------------------------------------------------------------------------
さらに用途別にアプリケーションという単位を作ります・・・無駄に階層化するところは好かんですね

Apacheの設定
--------------------------------------------------------------------------------
作ったプロジェクトにあわせてApacheの設定をカスタマイズします。

Apacheのmod_rewriteをオンにする

```
LoadModule rewrite_module modules/mod_rewrite.so
```

適当に設定します

``` apache
#symfony
<VirtualHost *:80>
  ServerName hogehoge
  DocumentRoot "D:\hogehoge\web"
  DirectoryIndex index.php
  Alias /sf "D:\php\PEAR\data\symfony\web\sf"
  <Directory "D:\php\PEAR\data\symfony\web\sf">
    AllowOverride All
    Allow from All
  </Directory>
  <Directory "D:\hogehoge\web">
    AllowOverride All
    Allow from All
  </Directory>
</VirtualHost>
```

既存環境と同居したい場合は hosts ファイルに別名を定義して

```apache
NameVirtualHost *:80

<VirtualHost *:80>
  ServerName localhost
  DocumentRoot "D:\Apache\htdocs"
</VirtualHost>
```

と名前ベースでのVirtualHostを構築してやればいいでしょう

モジュール作成
--------------------------------------------------------------------------------
symfonyでは一連の処理の流れをモジュールという塊で管理する
プロジェクトのディレクトリに移動して、コマンドドン

```
$ symfony generate:module piyo fuga
```

piyoアプリの中にfugaモジュールを作りました。

アクションクラスの記述
--------------------------------------------------------------------------------
↑モジュール作成で処理の流れの雛形がまとめてできるのでそこでActionクラスを記述する。



ビューの記述
--------------------------------------------------------------------------------
symfonyではビューはテンプレートに記述する。



