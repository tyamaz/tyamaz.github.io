---
title: Symfony1.2_テンプレート
aliases:
  - Symfony1.2_テンプレート
tags:
  - d/2009/01/06
  - n/PGM/PHP/Symfony/v1.2/テンプレート
  - m/未完
---

- 2009年01月06日
- symfony1.2


ヘッダ情報を生成する
================================================================================
ヘッダ情報はテンプレート中で

```php
<?php include_http_metas() ?>
```

このようにすることで出力できる。その内容は `app/hoge/config/view.yml` に基づく

`view.yml` には

```yml
  http_metas:
    content-type: text/html
    content-style-type: text/css
    content-script-type: text/javascript
```

このように記述すると

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<meta http-equiv="Content-Style-Type" content="text/css" />
<meta http-equiv="Content-Script-Type" content="text/javascript" />
```

このように出力される




他のActionへのURLを取得する
================================================================================

```php
url_for('モジュール名/Action名');
```

他のActionへのリンクを生成する
================================================================================
