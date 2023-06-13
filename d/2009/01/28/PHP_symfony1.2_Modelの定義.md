---
title: PHP symfony1.2 Modelの定義
aliases:
  - PHP_symfony1.2_Modelの定義
tags:
  - d/2009/01/28
  - n/PGM/PHP/Symfony/v1.2/Model
---

- 2009年01月28日
- symfony1.2


普通に作る
================================================================================
この `propel` は接続を表す。通常このままでOK

次の階層がテーブル名になる。全部小文字のアンダーバーつなぎが無難

phpNameで指定されているのがsymfony側で生成するモデル名になる

```yaml
propel:
  user:
    _attributes: { phpName: User }
    id:
    name:       varchar(255)
    created_at:
```


    `id` と `created_at` は特別なカラムで `id` は自動シーケンス integer 型、`created_at` はタイムスタンプ型になる。

外部キーをつける（他テーブルID利用）
================================================================================
カラム名に `他テーブル_id` のような名前をつけて、無設定にするとその間に自動的に外部キーが張られる。

```yaml
propel:
  user_info:
    _attributes: { phpName: UserInfo }
    id:
    user_id:
    age:       integer
    created_at:
```

張った外部キーは

```php
$hoge->getUserRelatedByUserId()->getName()
```

のように使うことができる

外部キーをつける（明示的に指定）
================================================================================
2つのカラムに同じ外部キーをつけたい場合には明示的にしていする必要がある

```yaml
user_info:
  _attributes: { phpName: UserInfo }
  id:
  father: { type: integer, foreignTable: user, foreignReference: id }
  mother: { type: integer, foreignTable: user, foreignReference: id }
  created_at:
```

こんな感じ

ID のカラムを bigint で定義する
================================================================================
こんな風に全部明示的に記述するとできます。

```yaml
propel:
  user:
    _attributes: { phpName: User }
    id: { type: bigint, required: true, primaryKey: true, autoIncrement: true }
    name:       varchar(255)
    created_at:
```

`1.0` のころは bigint 型で定義していると `order by` の順が狂うという話みたいでしたけど `1.2` では普通に `order by` できました。



