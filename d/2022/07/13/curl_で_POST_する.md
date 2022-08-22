---
title: curl で POST する
aliases:
  - curl_で_POST_する
tags:
  - d/2022/07/13
  - n/PGM/Linux/コマンド
  - n/PGM/ネットワーク/HTTP
---


このようにする。

```
$ curl -X POST -d "hogeId=12345" --data-urlencode "name=うんこ" --data-urlencode "content=なにか" -d "draft=false" "https://example.com/api/hogehoge"
```

送信するパラメータを付けるには `-d` オプションを使う、その中に `key=value` となるように1個ずつ書いていく。
複数あるなら複数回書けばいい。value が日本語のような URL Encode が必要なパラメータの場合 `--data-urlencode` オプションを使う


