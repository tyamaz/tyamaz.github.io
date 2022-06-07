---
title: Github を使ってみる（msysgit編）
aliases:
  - Github_を使ってみる（msysgit編）
tags:
  - d/2009/05/29
  - n/PGM/Git/Github
---


- ※ 古い情報です
- 2009年05月29日

アカウントを作る
================================================================================
作ってみます。 [Secure source code hosting and collaborative development - GitHub](http://github.com/) に行って `Pricing and Signup` をクリックしてみます。

`Open Source Free!` って書いてあるやつがあるから押す

- Username
- Email Address
- Password

を入力

keyの部分はとりあえず空欄にしておく

最後にボタンを押したらなんの確認も無くあっさり完成

SSHのキーを作る
================================================================================
[Providing your SSH Key - Guides - GitHub](http://github.com/guides/providing-your-ssh-key)

ここにmsysgit流のやり方がのってるので参考にしてみる。

git bashを起動して

```console
$ ssh-keygen -C “username@email.com”-t rsa
```

キーの保存位置を聞いてくるのでそのままエンターでデフォルトの位置にしておく
そうするとたぶん

```
C:\Documents and Settings\hoge\.ssh
```

ここらへんにキーができているので `id_rsa.pub` この内容を

```
https://github.com/account
```

このページ中の `SSH Public Keys` の部分にキーの追加があるので押すと `title` と `key` の入力欄が出てくるので、`title` は適当で、`key` には `id_rsa.pub` の中身をコピペする。

そうするとキーの登録が完了する。

リポジトリを作る
================================================================================
[Secure source code hosting and collaborative development - GitHub](http://github.com/repositories/new) ここで適当に作る。

ちょっと不安でしたが作ったらその後に説明が出てきたのでそれにしたがってみます。

```console
$ git config --global user.name "Your Name"
$ git config --global user.email hoge@gmail.com
```

まずユーザーの設定を書き換えろというみたいです。
もうすでに設定していたので、

```
C:\Documents and Settings\hoge\.gitconfig
```

ファイルを修正して対処します。

次にリポジトリに対応するものを作って、githubで作ったリポジトリに押し込むそうです

まず作ったリポジトリと同名のフォルダを作ります
このフォルダをカレントに git bash を起動して

```console
$ git init
```

で空のリポジトリを作るよ

その中に `index.html` みたいなファイル作るよ。

っで↑で作った自分のリポジトリにコミットするよ

```console
$ git add .
$ git commit
```

作った後の上部にプロジェクトのリポジトリが示されているのでそれを originって名前で登録しておく

```console
$ git remote add origin git@github.com:hoge/hogehogetestproject.git
```

そして今のローカルリポジトリをgithubへ押し込む

```console
$ git push origin master
```

最初の一回なので接続の確認があってパスフレーズを聞かれるので入力してOK

できた！




リポジトリを見てみる
================================================================================
[Your Dashboard - GitHub](https://github.com/) に戻って、右に自分のプロジェクトができているのがわかるのでクリックして覗いてみる

さっき登録した `index.html` があるのがわかる・・・

あとはローカルのリポジトリでコネコネやって、確定したら push を繰り返すだけだ。

