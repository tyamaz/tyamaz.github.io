---
title: MS-DOS_日記用バッチ
aliases:
  - MS-DOS_日記用バッチ
tags:
  - d/2010/07/16
  - n/PGM/Windows/MS-DOS
---

- 2010-07-16
- WindowsXP付属


日記用バッチ
================================================================================
年月フォルダを自動的に作って、日付の空のテキストファイルを作るバッチファイル

```
set year=%DATE:~0,4%
set month=%DATE:~5,2%
set day=%DATE:~8,2%
 
IF NOT EXIST %year% md %year%
IF NOT EXIST %year%\%month% md %year%\%month%
IF NOT EXIST %year%\%month%\%year%-%month%-%day%.txt copy nul %year%\%month%\%year%-%month%-%day%.txt
```

