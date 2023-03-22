---
title: Hadoop 環境構築（Windows編）
aliases:
  - Hadoop_環境構築（Windows編）
tags:
  - d/2009/09/07
  - n/PGM/Hadoop
  - n/PGM/Windows/WindowsXP/vSP3
  - n/PGM/Java/v1.6
---


- 2009年09月07日
- WindowsXP SP3
- Hadoop 0.18.3
- Java1.6

Javaのインストール
================================================================================
ダウンロード
--------------------------------------------------------------------------------
Linux版と同じで、ココ↓からダウンロードする

Java SE ダウンロード - Sun Developer Network (SDN) http://java.sun.com/javase/ja/6/download.html

バージョンもLinux版と同じで 1.6 を使いました。

インストール
--------------------------------------------------------------------------------
jdkとjreを

```
 D:\Java\jdk
 D:\Java\jre
```

にインストール

インストールできたかコマンドでチェック

```
$ java -version
```

Cygwinのインストール
================================================================================
HadoopはWindowsでの動作の仕組みを用意しておらず、Unixのシェル＋Javaという組み合わせで動作するようになっている。なのでWindowsで動かすにはそのドライブする仕組みとしてCygwinが必要になる

サイトに行ってセットアップのバイナリを落とす http://cygwin.com/

```
Cygwin! Install or update now!
```

なんてリンクがあるのでここから `setup.exe` をダウンロードする

```
実行する
 Install from internet
```

をチェック

`D:\cygwin` あたりにインストールすることにしてあとはデフォルト

```
 Select Local Package Directory
```

と聞かれるので `D:\cygwin_temp` あたりを勝手に作っておく。
これはダウンロードしたファイルの一時的な置き場所。

ダウンロード場所を選べといわれるので、無抵抗になんたらjpを選ぶ。

パッケージを選択するが面倒なので全部突っ込む。
cygwinインストール終了


Hadoopのインストール
================================================================================
ここからはLinux版と同じ

Hadoopのダウンロード
http://www.apache.org/dyn/closer.cgi/hadoop/core/

使うのはLinux版と同じ `0.18.3` これはLinux版とファイルもまったく同じ


解凍して `D:\hadoop` な感じで保存

環境変数の設定
--------------------------------------------------------------------------------
Linux版は↓こうだったので

```
export JAVA_HOME=/usr/java/latest
```

Windows版はCygwin環境を踏襲して

```
export JAVA_HOME=/cygdrive/d/Java/jre
```

として設定

ちなみにCygwinではローカルドライブは `/cygdrive` ディレクトリ以下にマウントされる

実行
================================================================================
Linux版同様にinputディレクトリを掘って中に適当な英文をぶっこむ。そしてコマンド一発

```
$ bin/hadoop jar hadoop-0.18.3-examples.jar wordcount input hogehoge
```

できたっぽい

結構あっさり

オリジナルのプログラムをコンパイル
================================================================================
これもLinux版と同様サンプルの `WordCount.java` を名前だけかえて `MyWordCount.java` を作る。
そしてコンパイル。

ここで注意が必要なのは、 javac は Cygwin から呼び出されるプログラムだが javac の動作は Windows に依存しているということだ。
つまり javac は Cygwin だとかいう馬の骨のことはなんも知らんわけだな。
なのでコンパイルはLinux版と違い

```
$ javac -cp "D:\hadoop\hadoop-0.18.3-core.jar" -d classes/ MyWordCount.java
```

このようにjavacへの引数だけWindows流儀にする　cygdriveから書いても何それなので注意

できたっぽい

jar化
================================================================================
ドン

```
$ jar -cvf MyWordCount.jar -C classes/ .
```

できたっぽい

オリジナルを実行
================================================================================

```
$ bin/hadoop jar mywordcount/MyWordCount.jar MyWordCount input hogehoge
```

動いた万歳！

これはLinux版と同様なので、Amazonに持っていっても動作します。

