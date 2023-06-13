---
title: PHP CakePHP1.2 DBを使う(MySQL編)
aliases:
  - PHP_CakePHP1.2_DBを使う(MySQL編)
tags:
  - d/2009/01/10
  - n/PGM/PHP/CakePHP/v1.2
---

- CakePHP 1.2.2

設定ファイルを書く
================================================================================
`hoge/app/config` 以下にある `database.php.default` というファイルをコピーして `database.php` という名前にしてから中にデータベースの接続情報を書き込む

ほとんどコメントのファイルで下のほうにチラッとだけ書くところがある。
普通に使うほうと、テスト用のDBと両方セットできるみたいだ。

名前はなんでもいいみたいだけどテスト用は明示的に頭に `test_piyo` みたいなものにしておいたほうがよさそうだ

設定がモロにPHPのクラスコードになっているのは好みですね。

```php
class DATABASE_CONFIG {
	var $default = array(
		'driver' => 'mysql',
		'persistent' => false,
		'host' => 'localhost',
		'login' => 'root',
		'password' => 'hoge',
		'database' => 'piyo',
		'prefix' => '',
                'encoding' => 'utf8'
	);

	var $test = array(
		'driver' => 'mysql',
		'persistent' => false,
		'host' => 'localhost',
		'login' => 'root',
		'password' => 'hoge',
		'database' => 'test_piyo',
		'prefix' => '',
                'encoding' => 'utf8'
	);
}
```

こんな感じだな

雛形のファイルには encoding が書いてないので、どう影響するかわからんが指定しておくほうがよさそうだ
