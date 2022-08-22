---
title: Git 一人で使ってみる（msysgit編）
aliases:
  - Git_一人で使ってみる（msysgit編）
tags:
  - d/2009/05/26
  - n/PGM/Windows/WindowsXP/SP3
  - n/PGM/VCS/Git/1.6.3
---

- 2009年05月26日
- WindowsXP SP3
- msysgit 1.6.3


インストール
================================================================================
インストールは [Git_インストール（WindowsXP編）](../10/Git_インストール（WindowsXP編）.md) ←ココを参考に

Git bash
================================================================================
msysgit には git を使う専用のコマンドラインのシェルの git bash というものがくっついてきてこれで Unix ライクに操作することができる。

普通にインストールするとフォルダに対して右クリックで git bash を対象のフォルダをカレントとして開くことができる。

この git bash は git 専用なので tab をバシバシ押すだけで、コマンドとか引数を補完してくれます便利

サンプルプロジェクト
================================================================================
ホームページの管理をするとして、サンプルのページを作る
Windowsによくありげな状況を想定して `My Document\hp\hp001\` なディレクトリを作り中に `index.html` を作る。内容は適当

```html
<html>
<head>
<title>git</title>
</head>
<body>
<h1>はじめてのGit</h1>
<p>はじめます</p>
</body>
</html>
</html>
```

こんな感じでいいでしょう。

文字コードは UTF-8 にして改行コードは LF に統一しておきます。


リポジトリの作成
================================================================================
なのでこの hp001 フォルダを右クリックし、このフォルダをカレントにした git bash を起動します。

そこで、コマンドドン

```console
$ git init
```

これでフォルダの中に `.git` というフォルダが作られたと思います。これがリポジトリになります。

うーん・・・このGitというものは今までのバージョン管理システムのようにサーバではなくて、単体のアプリのように動作するようですね。

自分を確認してみる
================================================================================
またもコマンドドン

```console
$ git var -l

core.symlinks=false
core.autocrlf=input
color.diff=auto
pack.packsizelimit=2g
help.format=html
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
GIT_COMMITTER_IDENT=unknown <hoge@.(none)> 1243329580 +0900
GIT_AUTHOR_IDENT=unknown <hoge@.(none)> 1243329580 +0900
```

次にコマンドを2発打ち込みます

```console
$ git config --global user.name "hoge"
$ git config --global user.email "hoge@hoge.com"
```

これでユーザーの設定ができました

```
C:\Documents and Settings\hoge
```

あたりに

```
.gitconfig
```

というファイルができて設定が保存されているはずです。
もう一度設定を見てみる。コマンドドン

```console
$ git var -l

core.symlinks=false
core.autocrlf=input
color.diff=auto
pack.packsizelimit=2g
help.format=html
user.name=hoge
user.email=hoge@hoge.com
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
GIT_COMMITTER_IDENT=hoge <hoge@hoge.com> 1243330219 +0900
GIT_AUTHOR_IDENT=hoge <hoge@hoge.com> 1243330219 +0900
```


ということで user 情報が追加されて、コミッターと作者の情報も追加されました。つまり自分がリポジトリをハンドリングする場合に、この情報が使われます。

コミットしてみる
================================================================================
さっきのは単にリポジトリという箱を作っただけでまだ中身は空っぽ、なのでここでコミットしてみます。

gitのコミットは2段階に分かれていて、対象の準備と、確定があります。まず

```console
$ git add .
```

で準備します。`add` のサブコマンドにドットの引数を与えます。これは配下のディレクトリを全部コミット対象として準備することを意味します。今回は `index.html`

そしてコミットします。

```console
$ git commit
```

を実行するとなんとviが立ち上がるので1行目にコミット用のコメントを入れて書き込み終了すると、コミットが完了します。今回は `hello git` と入れてみました。`vi` の使い方は適当に勉強してくださいｗ

面倒ですね・・・ということでこのviを秀丸にしてしまいましょう。
さっきの

```
.gitconfig
```

ファイルにこんな風に書き加えると秀丸でコミットのコメントが入力できるようになります。

```config
[core]
     editor = 'C:/Program Files/Hidemaru/Hidemaru.exe'
```

当然秀丸なので日本語入力でバシバシコメントが入れられます

コミットの履歴を見てみる
================================================================================
ここでコミットの履歴を見てみましょう

```console
$ git log
```

新しい順にコミットログが表示されます。でも日本語で入力したコミットコメントが読めません。

ここでちょっと環境を構築します。↓を参考にしてやります。

[WindowsでのGit環境構築とその注意点 - SourceForge.JP Magazine](http://sourceforge.jp/magazine/09/02/12/0530242/3)

まずページャを入れ替えます。これはmsysgitの付属のページャーが日本語に対応してないためなので、こいつを日本語用に入れ替えます。

[Less](http://www.greenwoodsoftware.com/less/index.html) ここらからダウンロードします。

解凍して中の `less.exe` を `C:\Program Files\Git\bin` の中の `less.exe`と入れ替えます。

これでマルチバイト文字は表示されるようになったんですが、コミットコメントは UTF-8 でされているのに、Windowsの端末の文字コードはSJISという扱いなので表示時に変換してやる必要があります。そこでnkfを使うみたいです。

解説のページはなにやら怪しいところからダウンロードしているが普通に落ちているので

[Vector：nkf.exe nkf32.dll Windows用 (Windows95/98/Me / ユーティリティ) - ソフトの詳細](http://www.vector.co.jp/soft/win95/util/se295331.html)

ここからダウンロードする。解凍して。中の `nkfwin\2090\win\nkf.exe` を、 `C:\Program Files\Git\bin` にコピーした。

`C:\Program Files\Git\etc` の `profile` というファイルのケツにこの1行を書き加える。

```sh
export GIT_PAGER="nkf -s | less"
```

これでログが日本語で読めるようになりましたね。


差分を見てみる
================================================================================
作った `index.html` を変更してみて

```console
$ git log
```

とコマンドを打つと変更差分を見ることができる。

ここでもグラフィカルに見たいのでちょっと設定してみましょう。↓を参考に設定してみます。

[dave^2 = -1: Setting up diff and merge tools for Git on Windows](http://www.davesquared.net/2009/02/setting-up-diff-and-merge-tools-for-git.html)

まず、このフォルダに `C:\Program Files\Git\cmd` バッチファイルを作って置きます `git-diff-wrapper.sh`

中身はこんなのにしときます。

```sh
#!/bin/sh
'C:/Program Files/WinMerge/WinMergeU.exe' $2 $5 | cat
```

私は WinMerge が好きなので WinMerge です。コマンドラインから引数を取って起動できるやつならなんでも使えそうです。

WinMerge 自体をダウンロードします。インストーラー版でバシバシやってインストールします。

最後におなじみの ``.gitconfig` ファイルにこんな風に書き込みます

```config
[diff]
    external = C:/Program Files/Git/cmd/git-diff-wrapper.sh
```

参考サイトの通りにやって

```
external diff died, stopping at index.html
```

なんてエラーメッセージが出る人は、↑の例を元にシングルクォートとかダブルクォートの有無をチェックしてみてください、必要の無いところにクォートを付けると、うまく WinMerge が起動してくれません。

設定後 git bash を再起動して

```
git diff
```

を実行すると、WinMerge が起動して、左がリポジトリ、右がワークという形で表示されます。バンザーイ

管理ファイルを増やしてみる
================================================================================
CSS ファイルを書いてみて管理対象を増やしてみましょう。

`css` ディレクトリを新たに掘って、その中に適当に `hoge.css` ファイルを作ります。スタイルは好みで付けときましょう
 
 ```
 css/hoge.css
 ```

そこで一発ステータスを見てみます

```
$ git status

 # On branch master
 # Changed but not updated:
 #   (use "git add <file>..." to update what will be committed)
 #   (use "git checkout -- <file>..." to discard changes in working directory)
 #
 #       modified:   index.html
 #
 # Untracked files:
 #   (use "git add <file>..." to include in what will be committed)
 #
 #       css/
 no changes added to commit (use "git add" and/or "git commit -a")

```

「index.htmlは変更されているよ」と表示されて、管理外のファイルとしてcssフォルダが登場しています。中の `hoge.css` に関してはまったく表示されていません。

こいつをコミットするためにAオプションを付けてサブコマンドaddを実行してみます。

``` console
$ git add -A
```

この後にもう一度ステータスを確認すると・・・

```console
$ git status
 # On branch master
 # Changes to be committed:
 #   (use "git reset HEAD <file>..." to unstage)
 #
 #       new file:   css/hoge.css
 #       modified:   index.html
 #
```

ばっちりですね。
コミットします。

```console
$ git commit
```

Aオプションは管理外になっているファイルを一発でaddしてくれるオプションです。

やったあとに、cssディレクトリの中を覗いてみましょう。普通に `hoge.css` ファイルがあるだけですね。

git ではフォルダ毎にメタ情報が入ったファイルをおくことはしないみたいです。一番トップにあるリポジトリが本当にすべてだということですね。

こりゃファイルの管理は楽だ。

戻してみる
================================================================================
ではバージョン管理の肝。巻き戻しをやってみましょう。

ではワーク側のファイルを編集してみて、とりあえず台無しにしてみます。編集して保存します。

元に戻してみます。Git では常にリポジトリと対象との状態の比較によって管理がなりたっていて、別に特別なメタ情報で連続性を管理しているわけではないみたいです。だから単純に古いリポジトリから、ワークツリーをあらたに作るだけで巻き戻しは完了します。

コミットした最新のものに巻き戻してみます

```console
$ git checkout HEAD index.html
```

これで `index.html` がコミット時点のファイルに巻き戻りました。

ここでコミットログを表示するとわかりますが
 
```console
$ git log
```

巻き戻ったのはあくまでワークツリーであってリポジトリはコミットされたままを保っています。

ではさらに古いリポジトリに巻き戻してみます。

```
$ git log
```

でコミットログを表示すると

```
commit 9796171a471a08e9500cd9c23e70119e628cb9a7
Author: hoge <hoge@hoge.com>
Date:   Tue May 26 19:58:55 2009 +0900

    hello git 3
```

こんな風に commit の後にハッシュ値が表示されます。Git では他のバージョン管理システムではリビジョンナンバーと呼ばれるようなコミット状態に付けられている番号がハッシュになっています。（中央が無いから一貫した番号が意味をなさないため）

こいつを指定して `checkout` することにより、任意の状態に戻れます。

```console
$ git checkout 9796171a471a08e9500cd9c23e70119e628cb9a7 index.html
```

長々と書いていますが、コミットを一意に決定できれば全部書く必要はないです

```console
$ git checkout 97961 index.html
```

こんな感じでも通ります。

ログを検索する
================================================================================
コミットコメントで検索
--------------------------------------------------------------------------------
コミットコメントで検索してみる。

```console
$ git log --grep="hoge"
```

できた。

日本語のコミットコメントで検索してみる・・・git bash は標準状態では日本語は入力できないようです。

再びココを参考に

[WindowsでのGit環境構築とその注意点 - SourceForge.JP Magazine](http://sourceforge.jp/magazine/09/02/12/0530242/3)

設定してみる。

```
C:\Program Files\Git\etc
```

このディレクトリに `inputrc` というファイルを用意して中に↓を書き込むようです。

```
set convert-meta off
set meta-flag on
set output-meta on
set kanji-code utf-8
```

とにかく書けばIMEの切り替えができるようになりそうです。git bash を立ち上げなおします。

確かに日本語入力できるようになったんですが・・・

```console
$ git log --grep="日本語"
```

とやってもコミットコメントに合致しません・・だめなのかな

ブランチを作る
================================================================================
まず適当に変更とコミットを繰り返してリポジトリにいくつかコミットを溜め込んでおきます。

ブランチを作ります。ブランチはコミットに付けるラベルみたいなもので

```console
$ git checkout -b hoge
```

とかで作れます。作った段階ではまだ何もおきてません。この段階では master の HEAD もブランチ hoge の HEAD も同じ状態です。ブランチは自分で作らなくても存在していて、それが今まで使っていた master ブランチというブランチです。

checkout に bオプション をつけてブランチを作成すると、作成後にそのブランチに自動的に移動します。

なので今いるブランチは hoge ブランチです。ワークツリーの中のファイルを変更して、今まで通りコミットします。

ここで分岐します。hoge ブランチの HEAD は master ブランチより枝分かれして作られ、まったく別物として進行していきます。

現在は hoge ブランチにいるので master ブランチにもどり作業します

```console
$ git checkout master
```

これで master ブランチに戻ります。

ワークツリーの内容が、hoge ブランチの分岐以前にもどっていることが確認できることからも、master ブランチに戻ったということがわかります。

このようにファイルそのもの変更履歴の管理というよりもファイル群状態そのものの管理をしているのが Git 流のようです。

ここでまたファイルに変更を加えてコミットしてみます。現在は master ブランチにいるので、コミット先は master ブランチとなり、hoge ブランチとは完全に枝分かれして別物として進んでいきます。

ここでまた hoge ブランチに移動してみます。

```console
$ git checkout hoge
```

ワークツリーのファイルを見ると、さっきの変更が無くなって、hoge ブランチの HEAD 状態で復帰しています。ここでは HEAD状態で復帰します。逆に言うと、コミットしない中途半端なファイルがあるとブランチの移動ができません。



ブランチをマージする
================================================================================
ブランチをマージするには本流となるブランチに移動して

```console
$ git checkout master
```

合流させるブランチを指定してマージします。

```console
$ git merge hoge
```

分岐した2つの流れで別々に編集したファイルがあるならば、当然衝突が起こるので回避しなければなりません。

変更箇所が master と hoge でまったく違うなら自動的にマージされて勝手にコミットされるわけですが、同じようなところを修正していた場合、衝突が起こります。

衝突したファイルには衝突しているであろう箇所に本流と合流対象のブランチの値が両方マーカー付きで入っています。ここを適当にエディタで編集して、OKならばコミットします。これでマージとしてコミットが実行されてmasterへのマージは完了です。


運用をすこし考えると、ブランチは小変更を短い期間で行うような用途に向いているような気がします・・・っというかマージという行為自体が大変なのであまりでかい変更を長い期間離れ離れにして更新すべきではないということですね。


作業用リポジトリを作る（ローカルtoローカル編）
================================================================================
ここは分散の本領発揮です。作業用リポジトリは本体のコピーってことで、適当にディレクトリpiyoを作って、hogeディレクトリにあるリポジトリをコピーします
つまり今は

```
 hoge/
  +--- index.html
  +--- .git/
 piyo/
```

こんな感じです。

ここでコマンドドン

```console
$ git clone hoge piyo
```

確認すると

```
 hoge/
  +--- index.html
  +--- .git/

 piyo/
  +--- index.html
  +--- .git/
```

ってな感じで、コピーが確認できました。この piyo を丸々USBメモリとかに突っ込んで持ち歩けば、オリジナルのhogeが無くても開発が継続できますね。

リポジトリ間マージ（ローカルtoローカル編）
================================================================================
次にコピーした piyo で作業してそれを hoge にマージしてみます。これはつまり共同開発しているのと同じことです。

piyo をすこし変更してコミットを増やしておきます。

変更をオリジナルの hoge に合流させてみましょう。piyo側で・・・

```console
$ git push
```

何も設定してないのにできるのは、clone した段階でオリジナルとのつながりが設定ファイルに記録されているからなのね

```
.git/config
```

の中に書かれている。

そうするとイロイロ怒られた末に push が成功する。

ではオリジナルの hoge リポジトリのほうに目を向けて、中を見てみよう・・・・・・あれ？変わってない？リポジトリを見てみると、clone 前の hoge の先っぽに clone 後の piyo のリポジトリをドッキングしたようなコミット群が形成されている。

参考サイト
================================================================================
- [Gitを使いこなすための20のコマンド - SourceForge.JP Magazine](http://sourceforge.jp/magazine/09/03/16/0831212)

