---
title: Android(開発)_Activity
aliases:
  - Android(開発)_Activity
tags:
  - d/2010/03/17
  - n/PGM/Android/dev
---


- 2010-03-17

Activityとは
================================================================================
画面の機能と状態をオブジェクト化したもの。

この「状態」を保持しているというのがActivityがWeb系のフレームワークのコントローラーとは違う部分。

Activityとhistory stack
================================================================================
Android では標準の UI で「戻るボタン」がある。
戻るというのは画面状態の巻き戻しを意味するわけだが、ここで Android では Activity の状態を巻き戻すのではなく、Activityが生成された場合（主に画面遷移）、前回のActivityをそのままの状態でオブジェクト丸ごと history stack という Activity のコレクションに格納する。

この時点で格納された Activity オブジェクトはユーザーの操作から切り離される。つまり Activity はもう変化しない。

Activityをintentを使い

```
HogeActivity → PiyoActivity → HogeActivity
```

順番に画面遷移させたとすると、最初の HogeActivity と最後の HogeActivity はまったく別物としてインスタンス化されるということ

戻るという行為の再現をオブジェクト状態のパラメータ変化ではなく、オブジェクトのスナップショットの蓄積という富豪的な実装にしているということ

この history stack を Android の解説の文献では「タスク」と呼んだりしています。よく Android で言っているマルチタスクのタスクとは違うのでそれはそれで注意

mement パターンのフレームワーク全体での実装という位置づけって感じですか


Activity 生成のオプションによる微妙なコントロール
================================================================================
Activity の生成は一定のルールを使って若干のコントロールができるようになっている。それは Manifest ファイルの「activity」要素の「android:launchMod」属性によってコントロールできる。設定できる値は


- standard（デフォルト）
- singleTop
- singleTask
- singleInstance

詳しくは
開発の基礎 Android Developers http://developer.android.com/intl/ja/guide/topics/fundamentals.html#lmodes]]
このページの内容は https://boo-yakinikunotare.ssl-lolipop.jp/orebase2/android_dev/activity に移動しました。
*目次
#contents

*バージョンと製造年月日
2010-03-17

*Activityとは [#dc8798cb]
画面の機能と状態をオブジェクト化したもの。

この「状態」を保持しているというのがActivityがWeb系のフレームワークのコントローラーとは違う部分。

*Activityとhistory stack
Androidでは標準のUIで「戻るボタン」がある。戻るというのは画面状態の巻き戻しを意味するわけだが、ここでAndroidではActivityの状態を巻き戻すのではなく、Activityが生成された場合（主に画面遷移）、前回のActivityをそのままの状態でオブジェクト丸ごとhistory stackというActivityのコレクションに格納する。

この時点で格納されたActivityオブジェクトはユーザーの操作から切り離される。つまりActivityはもう変化しない。

Activityをintentを使い
 HogeActivity → PiyoActivity → HogeActivity
順番に画面遷移させたとすると、最初のHogeActivityと最後のHogeActivityはまったく別物としてインスタンス化されるということ

戻るという行為の再現をオブジェクト状態のパラメータ変化ではなく、オブジェクトのスナップショットの蓄積という富豪的な実装にしているということ

このhistory stackをAndroidの解説の文献では「タスク」と呼んだりしています。よくAndroidで言っているマルチタスクのタスクとは違うのでそれはそれで注意

mementパターンのフレームワーク全体での実装という位置づけって感じですか。









*Activity生成のオプションによる微妙なコントロール [#f7bab32a]
Activityの生成は一定のルールを使って若干のコントロールができるようになっている。それはManifestファイルの「activity」要素の「android:launchMod」属性によってコントロールできる。設定できる値は
-standard（デフォルト）
-singleTop
-singleTask
-singleInstance

詳しくは
開発の基礎 | Android Developers http://developer.android.com/intl/ja/guide/topics/fundamentals.html#lmodes

この生成のルールはIntentのフラグとの組み合わせによっても変化します。
Intent | Android Developers http://developer.android.com/intl/ja/reference/android/content/Intent.html


じゃんじゃん生成じゃんじゃん積み上げ
--------------------------------------------------------------------------------
standard に設定すると、タスクを生成せず既存のタスクの一番上に Activity を新たに生成して積み上げます。これが標準の動き

つまりまったく launchMod を設定しなければ、遷移のたびに Activity が何度でも生成されそのhistoryが一直線にダラダラとつみあがっていく。

Android のリソースが潤沢で何も問題がおきないならこれでほぼOKだけど、リソース開放や大量のオブジェクトを保持する場合はうまくコントロールしてやらないと原因不明の問題が起きる場合アリ

じゃんじゃん生成じゃんじゃん積み上げだけど1個前は見るよ
--------------------------------------------------------------------------------
singleTop にすると standart とほぼ同じ動きなのだが生成予定の Activity が1個前の Activity と同じならばそれをスタック位置ごと再利用してしまうということ。

つまり intent 指定で Activity を起動したとしても、1個前に同じ Activity クラスから生成された Activity があった場合、変化しないということ。これを指定すると history stack 上に同じ画面が連続で登場しなくなるということ。

ってか同一画面に遷移することなんてあんのかよ・・・

