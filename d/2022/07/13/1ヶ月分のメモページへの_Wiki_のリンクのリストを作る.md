---
title: 1ヶ月分のメモページへの Wiki のリンクのリストを作る
aliases:
  - 1ヶ月分のメモページへの_Wiki_のリンクのリストを作る
tags:
  - d/2022/07/13
  - n/PGM/Linux/シェルスクリプト
---

如何様にもやり方はあるが、どういう流れで考えていくかのメモ。
これがベストだとかスマートだとかそういう話ではない。

最終的に欲しいもの

```
- [[hoge/piyo/fuga/2022/07/01]]
- [[hoge/piyo/fuga/2022/07/02]]
- [[hoge/piyo/fuga/2022/07/03]]
- [[hoge/piyo/fuga/2022/07/04]]
- [[hoge/piyo/fuga/2022/07/05]]
...
- [[hoge/piyo/fuga/2022/07/31]]
```

こういうやつ。

とりあえず 1 から 31 までの数が欲しいくそれベースで回したいのでまずそれ。

```
$ seq 31
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
```

簡単である。ゼロ埋めしたいのでマニュアル見る。

```
$ man seq
...
       -w, --equal-width
              equalize width by padding with leading zeroes
...              
```

らしい。

```
$ seq -w 31
01
02
03
04
05
06
07
08
09
10
...
30
31
```


できた。

あとはここに前後の装飾をドッキングすれば完了である。

と思ってたら、マニュアルにもっといいやつが書いてある

```
       -f, --format=FORMAT
              use printf style floating-point FORMAT
```

これを指定してとりあえず 2桁 ゼロ埋めで表示させる

```
$ seq -f "%02g" 31
01
02
03
04
05
06
07
08
09
10
...
30
31
```

ここまでできたら後は補うだけ。

```
$ seq -f "- [[hoge/piyo/fuga/2022/07/%02g]]" 31
- [[hoge/piyo/fuga/2022/07/01]]
- [[hoge/piyo/fuga/2022/07/02]]
- [[hoge/piyo/fuga/2022/07/03]]
- [[hoge/piyo/fuga/2022/07/04]]
- [[hoge/piyo/fuga/2022/07/05]]
- [[hoge/piyo/fuga/2022/07/06]]
- [[hoge/piyo/fuga/2022/07/07]]
- [[hoge/piyo/fuga/2022/07/08]]
- [[hoge/piyo/fuga/2022/07/09]]
- [[hoge/piyo/fuga/2022/07/10]]
- [[hoge/piyo/fuga/2022/07/11]]
- [[hoge/piyo/fuga/2022/07/12]]
- [[hoge/piyo/fuga/2022/07/13]]
- [[hoge/piyo/fuga/2022/07/14]]
- [[hoge/piyo/fuga/2022/07/15]]
- [[hoge/piyo/fuga/2022/07/16]]
- [[hoge/piyo/fuga/2022/07/17]]
- [[hoge/piyo/fuga/2022/07/18]]
- [[hoge/piyo/fuga/2022/07/19]]
- [[hoge/piyo/fuga/2022/07/20]]
- [[hoge/piyo/fuga/2022/07/21]]
- [[hoge/piyo/fuga/2022/07/22]]
- [[hoge/piyo/fuga/2022/07/23]]
- [[hoge/piyo/fuga/2022/07/24]]
- [[hoge/piyo/fuga/2022/07/25]]
- [[hoge/piyo/fuga/2022/07/26]]
- [[hoge/piyo/fuga/2022/07/27]]
- [[hoge/piyo/fuga/2022/07/28]]
- [[hoge/piyo/fuga/2022/07/29]]
- [[hoge/piyo/fuga/2022/07/30]]
- [[hoge/piyo/fuga/2022/07/31]]
```


できあがり。

