---
title: Google Chrome で特定のショートカットキーを無効にする
aliases:
  - Google_Chrome_で特定のショートカットキーを無効にする
tags:
  - d/2022/09/14
  - n/PGM/browser/Google_Chrome/v105
---


- Windows10
- Google Chrome v105
- Autohotkey


Autohotkey を使ってキーを無効化する
================================================================================
まず Google Chrome のアプリ名を調べる

Autohotkey の補助ツールの `Window Spy` を使って調べると、それが `chrome.exe` ということがわかった。



そしてスクリプトを書く

```autohotkey
; crhome の ctrl + p を無効化する 
#IfWinActive,ahk_exe chrome.exe
^p::
return
#IfWinActive
```

以上。






















