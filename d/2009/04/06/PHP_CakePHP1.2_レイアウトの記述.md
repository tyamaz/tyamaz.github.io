---
title: PHP CakePHP1.2 レイアウトの記述
aliases:
  - PHP_CakePHP1.2_レイアウトの記述
tags:
  - d/2009/04/06
  - n/PGM/PHP/CakePHP/v1.2
---

- 2009年04月06日
- CakePHP1.2


layoutファイルを作る
================================================================================
ファイルは `app/views/layouts` 以下に好きな名前で作る

`base.ctp` とかいう名前にします。

中身
================================================================================
中身は単なるPHPファイルなので適当に書いてくれ。

使える変数、メソッド
================================================================================
文字コード指定
--------------------------------------------------------------------------------
文字コードを指定を出力します head の直後あたりに突っ込んどくのが定番でしょう

```php
<?php echo $html->charset(); ?>
``

```xml
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
```

タイトル
--------------------------------------------------------------------------------
```php
<?php echo $title_for_layout; ?>
```

デフォルトでは controller 名が出力されるっぽいね

任意のタイトルを出したい場合はアクション無いとか、テンプレート内で

```php
<?php $this->pageTitle = "aaa" ?>
```

pageTitleってところに突っ込んでおけば出る。
テンプレのほうで宣言的に書いておくか、クラスのメンバ宣言のところで固定的に書いておくのがベターだと思う

css
--------------------------------------------------------------------------------

```php
<?echo $scripts_for_layout; ?>
```
cssのローディング記述は画面ごとに個別なことがおおいため、View側を先に評価して、後でlayout側に反映させることが可能になってます。

っでview側で

```php
<?php $html->css('hoge', null, null,false); ?>
```

って書くと(最後のfalseがポイント)layout側にその内容が出力されます。
※ネット上にechoが入っているサンプルコードがたくさん載ってますがechoは不要です。

複数のCSSを読み込みたい場合は２回書くとか配列で渡すとかできます。

Javascript
--------------------------------------------------------------------------------
Javascript も css とほぼ同様です。view側で

```php
<?php $javascript->link(array('hoge', 'piyo') ,false); ?>
```

のような感じで読み込みます。
そすうると layout 側の

```php
<?echo $scripts_for_layout; ?>
```

で spcrit タグの src 属性としてパスが補完されて展開されます。

こいつは JavascriptHelper なので controller で設定の記述が必要になります。


コンテンツそのもの
--------------------------------------------------------------------------------
テンプレートで出力されたコンテンツそのものがここに挿入されます

```php
<?php echo $content_for_layout; ?>
```


element
================================================================================
コンテンツ本体とは別の、全画面で使う共通項目みたいなものはCakePHPではelementという仕組みで実現する

たとえばメニューとかヘッダーとかフッターとかそういうやつ

elementは基本的にはテンプレートとかレイアウトと同じctpファイルとして書きます。

```
views/elements/hoge.ctp
```

みたいな感じで保存して

別の ctp ファイルから

```php
<?=$this->renderElement('hoge'); ?>
```

こんな感じで呼び出します。

呼び出し元はレイアウトでもテンプレートでもかまわないみたいね。そんで別にelementからelementを呼び出しもできる。記述方法はどれも変わらなくていけます。


コピペ用
================================================================================
雛形1
--------------------------------------------------------------------------------

```php
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <?php echo $html->charset(); ?>
  <title><?php echo $title_for_layout; ?></title>
  <meta http-equiv="Content-Style-Type" content="text/css" />
  <meta http-equiv="Content-Script-Type" content="text/javascript" />
  <?echo $scripts_for_layout; ?>
</head>
<body>
<?php echo $content_for_layout; ?>
</body>
</html>
```

