---
title: ポータブル版 VSCode をインストールする
aliases:
  - ポータブル版_VSCode_をインストールする
tags:
  - d/2022/07/20
  - n/PGM/editor/VSCode
  - n/PGM/Windows/Windows10
---

- 環境
  - Windows10
  - VSCode v1.69 インストーラー版インストール済み


ポータブル版のダウンロード
================================================================================
公式のダウンロードページから zip版 をダウンロードする。

- [Download Visual Studio Code \- Mac, Linux, Windows](https://code.visualstudio.com/download)


![](Pasted%20image%2020220720141804.png)


圧縮状態で 112MB そこそこデカい

今回使ったのは `1.69.2` というバージョン


インストール
================================================================================
解凍して、`C:\Users\hoge\00myapp\editor\vscode\ore1` 今回はこのフォルダに設置。
その後に `C:\Users\hoge\00myapp\editor\vscode\ore1\data` ディレクトリを作る。このポータブル版の設定はこの `data` ディレクトリに保存されることになる。

これでまっさらな VSCode が手に入ったので、用途毎に VSCode を使い分けたりできる。


バージョンアップ
================================================================================
ポータブル版にはインストール版のような自動アップデートの機能は無い。
なので data ディレクトリ以外を全部消してアップデートすることになる。

以下のような PowerShell のスクリプトが Windows用に提案されているらしく、それを使ってもよいようだ。

```powershell
# Remove temp file from portable user data
Remove-Item -Recurse -Force -Path "data/user-data" -Include @("Backups", "Cache", "CachedData", "GPUCache", "logs")
 
# Download latest stable build
curl.exe -L "https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-archive" -o stable.zip
 
# Delete anything except user data, update script and downloaded zip file
Get-ChildItem -Exclude @("data", "update.ps1", "stable.zip") | Remove-Item -Recurse -Force
 
# Unzip it
Expand-Archive -Path "stable.zip" -DestinationPath .
 
# Delete downloaded package
Remove-Item -Path "stable.zip"
 
Pause
```


コンテキストメニューに開くを追加する
================================================================================
今回設置したポータブル版の VSCode をエクスプローラーのコンテキストメニューに追加する

インストーラー版ではインストーラーが勝手にやってくれるのだがポータブル版ではそれが無いので自力で行う。

まず レジストリエディタ `regedit` を起動する。

左ペインに何か並んでいるので、↓のキーを探す

```
HKEY_CLASSES_ROOT\*\shell
```

おそらく一番上にある。その `shell` を右クリックして「新規→キー」として適当なキーを作る今回は `ore1vscode` とした。

そのキーをまた右クリックして「新規→キー」`command` を作る。

`command` を選択すると右ペインに「データ(値の設定なし)」と表示されているので、その名前の欄(`既定`)をダブルクリックする。「値のデータ」という日本語にするとやや意味不明な項目が入力できるポップアップが開くので、そこにさっきのポータブル版の VSCode のパスを入力する。

このように入力する

```
C:\Users\hoge\00myapp\editor\vscode\ore1\Code.exe "%1"
```

最後の `"%1"` パーセントイチ はファイル名がここに置き換わることを意味している。

これで OK

これでコンテキストメニューに `ore1vscode` が出るはず。


コンテキストメニューにアイコンをつける
================================================================================
コンテキストメニューにはインストーラー版にはアイコンが付いてわかりやすいので、ポータブル版にも付ける。

まずアイコンを用意する。VSCode アイコンで画像検索するとイロイロ出るので、その中にピンクのモノがあったので適当にダウンロードする。

こういうアイコンが作れるサイトがあるのでそこにアップロードして `icon.ico` ファイルを生成する。

- [ウインドウズ標準形式アイコン作成。無料で透過マルチアイコンが作れます。](https://ao-system.net/winicon/)

出来上がった `.ico` ファイルを適当な場所に保存する。今回は本体と同じ場所

```
C:\Users\hoge\00myapp\editor\vscode\ore1\icon.ico
```

に保存した。

レジストリエディタに戻り `ore1vscode` のキーに新規で「文字列値」を追加する。
そこでその名前を `Icon`、値を `C:\Users\hoge\00myapp\editor\vscode\ore1\icon.ico` で保存する。

これで設定したアイコンがコンテキストメニューに反映される。
































