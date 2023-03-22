---
title: PHP symfony1.2 Modelで検索
aliases:
  - PHP_symfony1.2_Modelで検索
tags:
  - d/2009/02/14
  - n/PGM/PHP/Symfony/v1.2
---

- 2009年02月14日
- symfony1.2

基本
================================================================================
検索には条件が必要です。symfony では条件と検索という行為が分離しています。条件はCriteriaクラスが担当することになっています。

無条件一件取得
================================================================================
SQLならば

```sql
select * from hoge limit 1
```

みたいなやつです。

```php
$cr = new Criteria();
$a = UserPeer::doSelectOne($cr);
```

合致しなければ null が返る

無条件全件取得
================================================================================

```php
$cr = new Criteria();
$a = UserPeer::doSelect($cr);
```

合致無しならば空の配列が戻る

無条件2件取得
--------------------------------------------------------------------------------
```php
$cr = new Criteria();
$cr->setLimit(2);
$a = UserPeer::doSelect($cr);
```

特定のカラム値に合致
================================================================================

```php
$cr = new Criteria();
$cr->add(UserPeer::ID, 2);
$a = UserPeer::doSelect($cr);
```

複数条件AND結合
================================================================================

```php
$cr = new Criteria();
$cr->add(UserPeer::AGE, 30, Criteria::GREATER_EQUAL);
$cr->add(UserPeer::ID, 1);
$a = UserPeer::doSelectOne($cr);
```

単にaddでズラズラ繋ぐ

基本的にはaddでズラズラ書けばいいのだけど、同一カラムに対してadd操作をやると条件が上書きされてしまう

なので、こういうときは add の代わりに　addAndを使う

複数条件OR結合
================================================================================
OR条件は少し特殊。ORは常にAND条件内にあるという前提になっていて、OR条件で結合するものは常にその結合の塊を新しいCriteria内で行う必要がある。

つまりCriteriaとは条件の塊の括弧を表すと思っていい

```php
//括弧生成
$cr = new Criteria();
//括弧の中身を生成
$cr1 = $cr->getNewCriterion(UserPeer::ID, 1);
$cr2 = $cr->getNewCriterion(UserPeer::ID, 2);
//括弧の中身同士でOR結合
$cr1->addOr($cr2);
//括弧に入れる
$cr->add($cr1);
//検索
$a = UserPeer::doSelect($cr);
```

複数OR条件のAND結合ならば
先にOR条件をズラズラつなげておいて、後でOR結合させた塊をaddで繋ぐことによってAND結合します。

```php
$cr = new Criteria();
	
$cr11 = $cr->getNewCriterion(UserPeer::ID, 1);
$cr12 = $cr->getNewCriterion(UserPeer::ID, 2);
$cr13 = $cr->getNewCriterion(UserPeer::ID, 3);
	
$cr11->addOr($cr12);
$cr11->addOr($cr13);
	
$cr21 = $cr->getNewCriterion(UserPeer::AGE, 10);
$cr22 = $cr->getNewCriterion(UserPeer::AGE, 50);
	
$cr21->addOr($cr22);
	
$cr->add($cr11);
$cr->add($cr21);
		
$a = UserPeer::doSelect($cr);
```

生成されるSQLは

```sql
SELECT
  user.ID,
  user.NAME,
  user.AGE,
  user.CREATED_AT
FROM
  `user`
WHERE
  ((user.ID=1 OR user.ID=2) OR user.ID=3) AND (user.AGE=10 OR user.AGE=50)
```


order byを指定する
================================================================================
昇順

```php
$cr->addAscendingOrderByColumn(UserPeer::AGE);
```

降順

```php
$cr->addDescendingOrderByColumn(UserPeer::AGE);
```

ランダム順

```php
$cr->addAscendingOrderByColumn('rand()');
```

検索条件に使う演算子
================================================================================

|定数|意味|例|
| --- | --- | ---- |
| Criteria::GREATER_EQUAL | 指定したカラムがパラメータ以上 | `ID >= ?` |


カラム数、カラム名がモデルと合わない結果を受け取る場合
================================================================================
いろいろグチャグチャやるとだめになりますね。
そこで

```php
$stmt = HogePeer::doSelectStmt($c);
$piyo = array();
while($res = $stmt->fetch()) {
  $piyo[] = $res;
}
```


なんてやると結果がカラム名をキーとした連想配列の配列で得ることができる。

※これはsymfony1.1とかだとdoSelectRSと呼ばれていたメソッドと同じものと考えてOK・・・かな




