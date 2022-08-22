---
title: Mybatis で resultMap で PK を ValueObject にマップして result を併用する場合の問題
aliases:
  - Mybatis で resultMap で PK を ValueObject にマップして result を併用する場合の問題
tags:
  - d/2022/07/05
  - n/PGM/Java/Mybatis
---

- [mybatis – MyBatis 3 \| Mapper XML Files](https://mybatis.org/mybatis-3/sqlmap-xml.html)

Mybatis の Mapper の XML の resultMap で PK を ValueObject にマップして result を併用する場合に SELECT 結果が変な風な挙動を見せる。その対処。

このように

```xml
<resultMap id="hogeMap" type="fuga.piyo.Hoge">
    <result property="name" column="name" />  
    <association property="id" javaType="fuga.piyo.HogeId">
        <id property="value" column="id" />  
    </association>
</resultMap>
```

DB のレコードに対して `Hoge` にマップし、その PK の `id` を ValueObject として `HogeId` にマップしたい。
ここで、テストデータを用意する

| id  | name |
| --- | ---- |
| 1   | taro |
| 2   | taro |
| 3   | jiro |

ID 違いで同名のレコード有り。

こうすると、DB 上はこの 3 レコードが当然挿入されるのだが、Mybatis 側で取得する(`findAll`的な)と、


| id  | name |
| --- | ---- |
| 2    | taro |
| 3    | jiro |

結果がこうなる。`name` が後勝ちになって1レコード(id=1)消失する。まるで `name` が PK になったかのような挙動。

これおそらくそもそも仕様に考慮が入ってなくて、OneToMany みたいな関係のことを考えて設計が進められたせいだと思われる。関係はどこかで最後は基本型に落ちるので、そこから遡ることで値は確定する。
しかしトップレベルはそれが無いので、なんだかしらないけど最初に書いてあるやつで確定させるっぽい。

これは、それを示唆する挙動があって、XML の定義として、`association` を `result` より先に書くことが出来ない。順番が関係するからなのだろう。

これを回避するには1番目で `id` PKであるということを明示すればよいとのことで、このように PK のフィールド指定を足してやればよい。

```xml
<resultMap id="hogeMap" type="fuga.piyo.Hoge">
    <id column="id" /><!-- これを足す -->
    <result property="name" column="name" />  
    <association property="id" javaType="fuga.piyo.HogeId">
        <id property="value" column="id" />  
    </association>
</resultMap>
```

`id` 要素は Mybatis にそれが PK であるという示唆をするためのタグ。機能としては `result` とほぼ同じ。

ポイントは `column` 指定はしているが、`property` 指定が無いということ。対応する Java側のメンバを指定しないということ。これにより、(SELECT の結果としての) `id` が PK であって `name` ではない、しかし、マップは無いからそういうことだよという解釈になるっぽい。

これは動きとしてはバグっぽいのだが、公式としては間違った挙動では無いっぽく、`id` つけてねということで実装には入ってない。

ここに何か書いてある [record missing in result · Issue \#1848 · mybatis/mybatis\-3 · GitHub](https://github.com/mybatis/mybatis-3/issues/1848)










