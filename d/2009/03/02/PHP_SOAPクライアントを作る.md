---
title: PHP SOAPクライアントを作る
aliases:
  - PHP_SOAPクライアントを作る
tags:
  - d/2009/03/02
  - n/PGM/PHP/v5.2
---

- 2009年03月02日
- PHP5.2.6
- CentOS5.2

SoapClient クラスを使う
================================================================================
まぁゼロから組み立ててもいいんですけど、WSDL から必要なインタフェースを構築するのが定番なのでこっちでやるのが楽勝です。5からは標準っぽいんで。

っで例のごとく使っている人が皆無ですね。世の中どうなってるんでしょうか？？？

インストール
--------------------------------------------------------------------------------
まずインスコですな

```console
$ yum search soap
```

なんてやると `php-soap` がいくつかヒットするので適当にドン

```console
$ yum insatall php-soap
```

`phpinfo` で入っていること確認


使ってみる
--------------------------------------------------------------------------------
まず WSDL の定義からクライアントを起こして

```php
$client = new SoapClient("http://hogehoge/piyo.wsdl");
```

ヘッダーの指定をする必要があるならしてしまう。普通ならなんか認証系の値だと思われる

```php
$wsdlNs = "http://hogehoge";
$idHeader = new SoapHeader($wsdlNs, "id", 'gegegege',false);
$passwordHeader = new SoapHeader($wsdlNs, "password", 'kekekeke',false);
$client->__setSoapHeaders(array($idHeader,
                                $passwordHeader));
```

そしてドン

メソッド自体（今ならばfuga）はWSDLから生成してるのでそのまま普通に使える。
パラメータは PHP の SoapClient 用の詳細マニュアルもサンプルコードも皆無なので生 SOAP の API マニュアルと WSDL の XML をにらめっこして妄想して作るとなんとなくうまくいくと思う

```php
$result = $client->fuga(array("paramname1" => array("paramnamefirst" => "aaa", 
                                                    "paramnamesecond" => "bbb",
                                                    "paramnamethird" => "ccc")));
```


https な WDSL を使う
================================================================================
https な SSL 通信が必要な場所の WDSL を取得するには、SSL モジュールが使えるようになってないと使えなくて、

```
SSL support is not available
```

こんなエラーが出たりしますね。

適当に入れてくださいｗｗ


