---
title: MediaWiki ロリポップに MediaWiki をインストールする
aliases:
  - MediaWiki_ロリポップに_MediaWiki_をインストールする
tags:
  - d/2009/09/26
  - n/PGM/MediaWiki/v1.15.1
  - n/PGM/PukiWiki
  - n/PGM/ホスティング/ロリポップ
---

- 2009年09月26日

PukiWiki から MediaWiki へ移行を考えて
================================================================================
なんだか PukiWiki がとっても古くなってきて心配になってきたので、メジャーで抜群の将来にわたって不安の無い MediaWiki に以降も考えてテスト運用も兼ねてインストールしてみた。

一応↓を参考にやったけど鵜呑みにしないほうがいい

Manual:Streamlined Windows Install Guide/ja - MediaWiki http://www.mediawiki.org/wiki/Manual:Streamlined_Windows_Install_Guide/ja


mediawikiのダウンロード
================================================================================
http://www.mediawiki.org/wiki/MediaWiki/ja

今回のバージョンは1.15.1

ロリポップにアップロード
================================================================================
解凍

適当なディレクトリにアップロード今回は `mediawiki01` というディレクトリアップロードした

データベースを作る
================================================================================
アップロードしている間にデータベースを確認する。ロリポップでは1ユーザーで1個しかデータベースを持てないみたいだな。
つまりスキーマの使い分けが事実上できないと・・・つまりたくさんアプリをインストールできないってことか

なんだか使いにくいな。一応テーブル名に MediaWiki 側がプレフィックスをつけれるようなので無理やりには共存可能。

とりあえずその1個しか作れないというデータベースをロリポップユーザー画面から作る

ロリポップでは phpMyAdmin 環境を提供してくれているのでそれで出来上がったデータベースを確認する。

そうこうやっている間にアップロード完了した。

パーミッションの変更
================================================================================
設定ファイルが生成されるっぽく、config ディレクトリに書き込み権限が必要らしいので 755 あたりにしとく

マニュアルには

> データベースのルートパスワードを知っている場合、MediaWikiインストールスクリプトは新しいデータベースを作成します。この場合、下記のインストールスクリプトの実行を省略することが出来ます。

なんて書いてあるが、データベース生成と設定ファイルの生成やテーブル作成は無関係ない。訳だけやって検証してねーのかよ！

ってことで省略できません。

設定スクリプトで設定
================================================================================
トップページに行くと

> お前PHP5だな

みたいな英語が出てきて、そっちのリンクに行くとガッツリインストールスクリプトが動作します。

Wiki の初期設定をイロイロ書き込む。メール関係は動作が怪しいからとりあえず全部オフにした。データベース設定はMySQLを選んでアカウントとDBとかはロリポップDBの画面に出ているやつにした。

そんでロリポップは2009年09月26日現在 MySQL4.0 で動いているのでそれを選ぶ

最後に完了みたいなボタンを押すと環境チェックが走ってそのあとにテーブルがズラズラ生成されるのでロリポップ側の phpMyAdminから確認する。たしかにできている。

そして自分の個人用設定が `config/LocalSetting.php` に吐き出された。
デフォルト設定をここでオーバーライドしているようだ。

設定
================================================================================
その後に MediaWiki を動作させると

> config/LocalSetting.phpを親のディレクトリに動かせ

みたいな英語が出てくるのでその通りに config ディレクトリからルートのディレクトリに移した。

っで一応動作完了

ユーザーの権限縛り
================================================================================
基本的に俺だけで使うので

`LocalSetting.php` に↓の設定を書く

```php
$wgGroupPermissions['*']['createaccount'] = false;
$wgGroupPermissions['*']['edit'] = false;
$wgGroupPermissions['user']['edit'] = true;
$wgGroupPermissions['sysop']['edit'] = true;
```

これの意味は

- 未ログインユーザーの新規アカウント作成禁止
  - 既存のユーザーしかアカウントを作れない、つまりアカウントは俺以外に作れない。そして俺はアカウントを増やす気は無い。つまりこれからあともアカウントは俺だけ
- 未ログインユーザーの編集禁止
  - そのまんま、そしてログインできるのは俺だけで、ユーザーは俺だけ。つまり編集できるのは俺だけ。MediaWikiのデフォルトでは編集権限を持っているのは未ログインユーザーとログインユーザーなので一方を禁止したってこと
- ログインユーザーの編集許可
  - これは一応デフォルトのようなのだけど明示的にオンにしておく
- 管理者ユーザーの編集許可
  - 唯一のユーザーが管理者ユーザーなのでこいつもオンにしておく


