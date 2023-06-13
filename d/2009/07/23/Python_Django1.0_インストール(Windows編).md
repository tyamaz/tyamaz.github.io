---
title: Python Django1.0 インストール(Windows編)
aliases:
  - Python_Django1.0_インストール(Windows編)
tags:
  - d/2009/07/23
  - n/PGM/Python/v2.6
  - n/PGM/Python/Django/v1.0
---

- 2009年07月23日
- Python 2.6.2
- Django 1.0.2

インストール
================================================================================
Django | How to install Django | Django Documentation http://docs.djangoproject.com/en/dev/topics/install/

ここを参考にしてやっていきます。


Pythonのインストール
================================================================================
まず Python をインストールします。注意書きを見ると 2.3 〜 2.6 のバージョンで動くよ〜とあるので Pyhton2.6.2 をダウンロードしてインストールします。

Python標準リリース http://www.python.jp/Zope/download/pythoncore

`D:\Python\` ここら辺にインストールした

インストーラー使ってもパスを設定してくれないようなので `D:\Python` にパスを通しておく

```
$ python --version
```

でインストール確認OK


*データベースのセットアップ [#u11e49dc]
「2.5以降ならばデータベースのセットアップは要らないよ」って書いてある。これはSQLiteが同梱されているからかな。今回は2.6.2なのでスキップする。


Djangoのインストール
================================================================================
ディストリビューションごとか、正式リリースか、開発版か？どれいれる？とある。
ディストリビューション版にはWindowsは無く、開発版はありえないので正式リリース版にする。

ここから Django | Download http://www.djangoproject.com/download/

`latest official version` を落とすことにする。今回は `1.0.2` を使う

解凍

インストール用のPythonスクリプトが同梱されているので実行する。Pythonはもうインストールしてありますからね。

```
$ python setup.py install
```

Python のインストール位置を認識しているらしく、今回 Python がインストールしてある `D:\Python` 以下の `Lib\site-packages\django\` この位置にファイルがドカドカコピーされる

つまり Django コマンド群を入れておいて、スクリプトを生成する rails チックな状態なんだな。
個人的にはコピーだけで完了する Cake 方式のほうが好み

コピーされたものを見るとそのままそっくりプロジェクトの雛形のようなのでそんなに違和感もないけどね

これでおわりっぽ。Python は自前でWebサーバを持ってるので、ほかのインストールはいらんのね。さすが電池入り。

コマンドラインで

```
$ python
```

と打って Python の対話モードに入り

```python
import django
```

と打って何もエラーが出なければインストール成功です。


