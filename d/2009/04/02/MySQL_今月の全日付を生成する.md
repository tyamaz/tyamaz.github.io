---
title: MySQL_今月の全日付を生成する
aliases:
  - MySQL_今月の全日付を生成する
tags:
  - d/2009/04/02
  - n/PGM/DB/MySQL/v5.0
---

- 2009年04月02日
- MySQL5.0

今月の全日付を生成するSQL
================================================================================
日付自体は関数で生成できるんですけど、SQLは駆動するためのレコードが必要なので駆動用のテーブルを適当に作ります。

```sql
CREATE TABLE IF NOT EXISTS `num` (
  `count` int(11) NOT NULL
) ENGINE=MyISAM DEFAULT CHARSET=latin1;
```


そこに駆動用のレコードを十分な量突っ込んでおきます。今回は1ヶ月分あればいいので31レコード以上あればOKです

```sql
INSERT INTO `num` (`count`) VALUES(0);
INSERT INTO `num` (`count`) VALUES(1);
INSERT INTO `num` (`count`) VALUES(2);
INSERT INTO `num` (`count`) VALUES(3);
INSERT INTO `num` (`count`) VALUES(4);
INSERT INTO `num` (`count`) VALUES(5);
INSERT INTO `num` (`count`) VALUES(6);
INSERT INTO `num` (`count`) VALUES(7);
INSERT INTO `num` (`count`) VALUES(8);
INSERT INTO `num` (`count`) VALUES(9);
INSERT INTO `num` (`count`) VALUES(10);
INSERT INTO `num` (`count`) VALUES(11);
INSERT INTO `num` (`count`) VALUES(12);
INSERT INTO `num` (`count`) VALUES(13);
INSERT INTO `num` (`count`) VALUES(14);
INSERT INTO `num` (`count`) VALUES(15);
INSERT INTO `num` (`count`) VALUES(16);
INSERT INTO `num` (`count`) VALUES(17);
INSERT INTO `num` (`count`) VALUES(18);
INSERT INTO `num` (`count`) VALUES(19);
INSERT INTO `num` (`count`) VALUES(20);
INSERT INTO `num` (`count`) VALUES(21);
INSERT INTO `num` (`count`) VALUES(22);
INSERT INTO `num` (`count`) VALUES(23);
INSERT INTO `num` (`count`) VALUES(24);
INSERT INTO `num` (`count`) VALUES(25);
INSERT INTO `num` (`count`) VALUES(26);
INSERT INTO `num` (`count`) VALUES(27);
INSERT INTO `num` (`count`) VALUES(28);
INSERT INTO `num` (`count`) VALUES(29);
INSERT INTO `num` (`count`) VALUES(31);
INSERT INTO `num` (`count`) VALUES(32);
INSERT INTO `num` (`count`) VALUES(33);
INSERT INTO `num` (`count`) VALUES(34);
INSERT INTO `num` (`count`) VALUES(35);
```

そんで↑のレコードを使って MySQL の関数をキックしていきます。
なんだか汚いですけどなんとか出ます

```sql
SELECT
    ADDDATE(DATE(DATE_FORMAT(NOW(), '%y-%m-01')), count)
FROM
    `num`
WHERE
    count < DAYOFMONTH((DATE(DATE_FORMAT(CURRENT_DATE(), '%y-%m-01')) + INTERVAL 1 MONTH) - INTERVAL 1 DAY)
ORDER BY
    count ASC
```

まず↑の核となるテーブルを呼び出し月末までループします。そのループ回数に制限をかけます今日が 04月10日 ならばループ回数を30回にしたいです。

そこで月末日付でレコード数を制限します。月末日付は月の初日に1ヶ月追加して1日戻すことで得ています。その日付部分だけ取り出してレコード内の数と比べています。
ここで4月の月末は30日なので30未満ということで count 値が0〜29のレコードで30回ループすることになります。

ここで月の初日を文字列的に勝手に生成して、その日付にcount値を足すことでカレンダーを生成します。
初日は一日なので最初のループではゼロが足されて初日、最後のレコードでは29が足されて30日という計算になります。

このテーブルをインラインビューにしてleft join すれば、日付的に歯抜けのレコードをビッチリ埋まったレコードに整形することができたりとかなんだかいろいろ便利です。
