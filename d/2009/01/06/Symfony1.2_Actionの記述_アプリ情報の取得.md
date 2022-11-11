---
title: Symfony1.2 Actionの記述 アプリ情報の取得
aliases:
  - Symfony1.2_Actionの記述_アプリ情報の取得
tags:
  - d/2009/01/06
  - n/PGM/PHP/Symfony/v1.2/Action
---

- 2009年01月06日
- symfony1.2

セッション情報の取得
================================================================================
セッションのオブジェクト自体は `getUser` で取得できる。
このオブジェクトからは `getAttribute` メソッドでキー指定で値を取り出すことができる。
第二引数はキーが存在しなかった場合に返す値。

```php
$session = $this->getUser();
$name = $session->getAttribute('name', '名無し');
```


モジュール名の取得
================================================================================
```php
$moduleName = $this->getModuleName();
```





