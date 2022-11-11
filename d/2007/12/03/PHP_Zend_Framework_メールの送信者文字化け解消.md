---
title: PHP Zend Framework メールの送信者文字化け解消
aliases:
  - PHP_Zend_Framework_メールの送信者文字化け解消
tags:
  - d/2007/12/03
  - n/PGM/PHP/Zend_Framework/1.0.3
---




- 2007年12月03日記述
- ver 1.0.3で確認してます


なにやら `Zend_Mail` コンポーネントにバグがあるようで
メーラーによっては送信者の欄が文字化けるようだ・・・

っと本にはバグと書いてあったがたぶん違う

内部エンコードを指定していないためと思われ。メールのヘッダー部のエンコードの前に

```php)
mb_internal_encoding('UTF-8');
```

こうしてやると文字化けしなくなる

