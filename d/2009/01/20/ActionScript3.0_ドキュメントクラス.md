
---
title: ActionScript3.0 ドキュメントクラス
aliases:
  - ActionScript3.0_ドキュメントクラス
tags:
  - d/2022/11/22
  - n/PGM/ActionScript/v3.0
  - n/PGM/Flash/CS3
---

- 2009年01月20日
- Flash CS3
- ActionScript3.0


ドキュメントクラスとは
================================================================================
swfが実行されたらステージ上のシンボルがインスタンス化された後に一番最初にインスタンス化されるオリジナルコードのエントリポイントとなるクラス。

つまりこのドキュメントクラスに指定しておいたクラスは勝手にコンストラクタが呼ばれる。

挙動的にはこのクラスのインスタンスは ActionScriot2.0 の root のように振舞う気がします。ハイ

インスタンスへのアクセスができる
================================================================================
ドキュメントクラスはFlash側でステージに配置したインスタンスにインスタンス名でアクセスすることができる・・・

逆に言うとドキュメントクラス以外はステージに配置したインスタンスに触れることができない・・・ちょっとこれは糞仕様なのじゃないの・・・

