---
title: Python2.5 日時操作
aliases:
  - Python2.5_日時操作
tags:
  - d/2010/02/23
  - n/PGM/Python/v2.5
  - n/PGM/Python/Ope/日時操作
---

- 2010年02月23日
- Python2.5

datetimeモジュール
================================================================================

```python
import datetime
```

Pythonでは日時操作に `datetime` モジュールを使う。
結構この辺に名前の統一感が無いので注意かも


日時を扱うクラス
================================================================================
日時を扱うのは `datetime` クラスだ。これモジュールとおんなじ名前で紛らわしいｗ


現在時刻を得る
================================================================================

```python
import datetime
datetime.datetime.now()
```

1時間加算する
================================================================================
時間計算をするときは時間の増減を表すクラス `timedelta` を使う

```python
import datetime
n = datetime.datetime.now()
n + datetime.timedelta(hours=1)
```




