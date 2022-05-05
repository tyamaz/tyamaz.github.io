---
title: USB / mini USB
---

USB端子の形状の1つ。フルサイズの USB 端子(Standard-A)よりもやや小さい。`micro USB` や `USB Type-C` の普及でほぼ使われることはなくなったが、一部の強度が必要かつ着脱回数が多いような状況の機器(電子楽器、自作キーボード)では現役で使われてたりする。ホスト側の `mini USB Type-A` 端子というものも一応は存在するが

端子が

| No. | memo              |
| --- | ----------------- |
| 1   | 電源プラス        |
| 2   | データマイナス    |
| 3   | データプラス      |
| 4   | ID                | 
| 5   | GND(電源マイナス) |


このようになっている。5本の端子があり、通常の USB 端子(Standard-A)よりも一本多く、 `ID` という端子が追加されている。こいつは通常は何も結線されてないが特殊な `OTGケーブル` という用途として使いたい場合隣の `GND` と合流させる。接続機器側が `GND` との合流を検知すると `OTG` 挙動になるという仕組み。単なるディップスイッチのような存在で信号的な意味は無い。

`OTG` というのは `USB On The Go` という機能で、USB 規格で接続できるデバイスが多様化する中でホスト無しで通信できてもいいのではという考えで拡張された規格になる。乱暴に言うと `Type-B` 端子同士で通信したいということになる。そのようなことができるケーブルを `OTGケーブル` と呼んでいて、上記のように `ID` 端子が `GND` に接続されている。このケーブルはそもそも両端が `B` ケーブルなので見ればすぐにわかる。






