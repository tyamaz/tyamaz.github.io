---
title: Hadoop 環境構築（Linux編）
aliases:
  - Hadoop_環境構築（Linux編）
tags:
  - d/2009/08/06
  - n/PGM/Hadoop
  - n/PGM/Linux/d/CentOS/v5.3
---

- 2009年08月06日
- CentOS5.3
- Hadoop 0.18.3

Hadoop環境構築メモ
================================================================================
vmware 上に vm として構築

CentOS5.3のインストール
================================================================================
標準でサーバーとかは入れていない
vmware-toolsをインストールしてホストのWindowsと楽に連携できるようにしておく


Java の準備
================================================================================
標準でopenjdkが入っているので消しておく

```sh
$ yum remove java
```

java を使うプログラムがごっそりなくなるけど気にしない。

Sun バージョンを使うので rpm をダウンロードします。

Java SE ダウンロード - Sun Developer Network (SDN)
http://java.sun.com/javase/ja/6/download.html

ここらへんから
Linux版をダウンロード これ `jdk-6u15-linux-i586-rpm.bin`

```
$ chmod +x jdk-6u15-linux-i586-rpm.bin
$ ./jdk-6u15-linux-i586-rpm.bin
```

バージョンチェック

```sh
$ java -version
java version "1.6.0_15"
Java(TM) SE Runtime Environment (build 1.6.0_15-b03)
Java HotSpot(TM) Client VM (build 14.1-b02, mixed mode, sharing)
```

インストールOK


Hadoop（mapreduse）のインストール
================================================================================

ここからダウンロード
Apache Download Mirrors - The Apache Software Foundation
http://www.apache.org/dyn/closer.cgi/hadoop/core/

amazonはバージョンが0.18っぽいので0.18を使う。今回は0.18.3 `hadoop-0.18.3.tar.gz`

```sh
$ tar xvzf hadoop-0.18.3.tar.gz
```

シンボリックリンクを作っておく

```
$ ln -s hadoop-0.18.3 hadoop
```

`hadoop/conf/hadoop-env.sh` この設定ファイルを編集する。環境変数を正しい値にセット

Sunのrpmを使った場合JAVAは

```
export JAVA_HOME=/usr/java/latest
```

この位置にインストールされるのでそうしておく

Hadoopの動作確認
サンプルを使って動作確認する

まず解析前のデータを入れておくディレクトリを作る
適当に

```sh
$ mkdir input
```

その中に適当なファイルを作って設置する

```
$ vim hoge1.txt
```

内容は

```
This is a pen. I am a boy.
```

もう一個作っておく

```
$ vim hoge2.txt
```

内容は

```
Is this a pen? Are you a teacher?
```

適当杉ｗ

```
 hadoop
   |
   +input
     |
     +-----hoge1.txt
     +-----hoge2.txt
```

こんな感じ

っでドン

```
$ bin/hadoop jar hadoop-0.18.3-examples.jar wordcount input hogehoge
```

ズラズラと結果がhogehogeディレクトリに出力されました。
 Are     1
 I       1
 Is      1
 This    1
 a       4
 am      1
 boy.    1
 is      1
 pen.    1
 pen?    1
 teacher?        1
 this    1
 you     1

動作完了OK



オリジナルのwordcountプログラムを作ってみる
================================================================================
サンプルは動きました。自作のwordcountのプログラムを作ってみます。

まずどこでもいいので適当に作業用のディレクトリを作ります。

```sh
$ mkdir mywordcount
```

とでもしておきます。

オリジナルのソースは、ここの中に転がっています。 `src/examples/org/apache/hadoop/examples`

この中の `WordCount.java` を改造します。さっき作った作業ディレクトリにコピーします。

編集します。はじめなので機能を変えずにコンパイルだけやって動作確認しようと思います。

`WordCount.java` を `MyWordCount.java` にリネームし、同時にクラス名もそれにあわせて変更します。当然使っている箇所が内部にもあるのでそれも変更します。パッケージの宣言は消しておきます。

それではコンパイルしてみます。コンパイルしたclassファイルを置くためのディレクトリを作っておきます

```
$ mkdir classes
```

そんでコンパイルします。

コンパイルするには hadoop の core がいるようなので cp オプションで、core のjar を指定してやります。コンパイル済みの class ファイル置き場を `d` オプションで指定、コンパイル対象の今回のファイルを最後に指定して。ドン

```
$ javac -cp ~/hadoop/hadoop-0.18.3-core.jar -d classes/ MyWordCount.java
```


出来上がりを確認して、こいつをjarに固めます

```
$ jar -cvf MyWordCount.jar -C classes/ .
```

無事 `MyWordCount.jar` が出来上がりました。
さて実行してみます

hadoopに

- jar
- クラス名
- 入力データが置いてあるディレクトリ
- 出力ディレクトリ

を与えて実行します。

```
$ bin/hadoop jar mywordcount/MyWordCount.jar MyWordCount input hogehoge
```

結果出ましたね。やった。


中を読んでみる
================================================================================
動いたので動作を確認しつつ動作させつつ改造してみます。

全体構成を眺めると・・・WordCount クラスがあって、その中に内部クラスとして

- MapClass
- ReduceClass

があり、あとはいくつかサブルーチン的なメソッドと、駆動させるためのエントリポイントが用意されているよう

MapClassクラスはMapperインターフェースを実装しています。

mapメソッドは

- 操作しているデータ行のポインタ
- 操作対象の行そのもの
- 出力オブジェクト（こいつが後でReduce操作に行く）
- 進行状況

が渡される予定
今回は単語を勘定するということで・・・

まず1行データの `Text` という謎型のインスタンスを `toString` メソッドで `String` に変換しています。

そんでこいつを単語毎に分割するために `StringTokenizer` インスタンスを新たに生成しています。
`split` メソッドが普通に実装されていないところに Java の歪みを感じますが、進みます

`StringTokenizer` インスタンスは文字列１個をコンストラクタに渡して生成すると、内部で、空白文字、タブ文字、改行文字、復帰改行文字を区切り文字として文字列を分解します。

分解した単語を取り出しながら出力オブジェクトに詰めていきます。
出力オブジェクトはTextとIntWritebleというオブジェクトをセットで詰めていくようです。

よくわからんのですがStringとintに対応するクラスのようで、比較系のメソッドが拡張されていました。

今回は単にレコードを足し上げるだけなので、このIntWritebleは常に固定です。

ためしにこいつの初期値を2にしてコンパイルして実行してみます。
1の場合の結果はと比べてみましょう
1の場合
 Are     1
 I       1
 Is      1
 This    1
 a       4
 am      1
 boy.    1
 is      1
 pen.    1
 pen?    1
 teacher?        1
 this    1
 you     1

2の場合
 Are     2
 I       2
 Is      2
 This    2
 a       8
 am      2
 boy.    2
 is      2
 pen.    2
 pen?    2
 teacher?        2
 this    2
 you     2

ってことでこの数値を足しあげているということがわかりました。
コレを踏まえてReduceを読んでみます。

Reduceクラスにはreduceメソッドがあり、こいつがさっきの出力オブジェクトを処理するようです。
引数は

- mapメソッドで出力したTextインスタンス1個分のデータ
- mapメソッドで出力したIntWritebleインスタンスのコレクション
- 結果への出力メソッド
- 進行状況

のようです。この引数からももうすでにmapに渡したTextインスタンスで値が押しつぶされているということが伺えます。

内部で全部足しあげて、結果出力コレクションへ追加しています。Textインスタンスのユニーク毎の処理になっています。


後は単なる駆動です
runメソッドを読んでみます。
ここではどんな形のデータを処理するかを記述するようです。文字列なのか数値なのかという感じですね。

この型に関しては `src/core/org/apache/hadoop/io` このパッケージ以下にイロイロ定義されているので読めばいいと思う。
チラ見したところbooleanとかArrayとかも使えそう

