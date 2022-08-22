---
title: Linux Mint 20.1 に PowerShell_をインストールする
aliases:
  - Linux_Mint_20.1_に_PowerShell_をインストールする
tags:
  - d/2022/07/18
  - n/PGM/Windows/PowerShell
  - n/PGM/Linux/d/Linux_Mint/20.1
---

Windows で使ってみてなかなか手応えが良かったので、Linux にも入れてみる。

Linux Mint 20.1 に対応する Ubuntu は 20.04 LTS。なので

```
https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
```

このパッケージを入れればよいようだ。

まず準備で、これらを入れる。

```
$ sudo apt-get install -y wget apt-transport-https software-properties-common
```

その後にパッケージを取ってくる

```
$ wget "https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb"
```

インストールする

```
$ sudo dpkg -i packages-microsoft-prod.deb
```

設定できたので、リポジトリを更新する

```
$ sudo apt-get update
```

PowerShell をインストールする

```
$ apt search powershell
p   powershell                      - PowerShell is an automation and configurat
p   powershell-lts                  - PowerShell is an automation and configurat
p   powershell-preview              - PowerShell is an automation and configurat
```


```
$ sudo apt install powershell
$ pwsh --version
PowerShell 7.2.5
```

古い版では `powershell` で起動するようなことが書かれているがこのバージョンでは `pwsh` で起動する。

インストール完了起動してみる


```
$ pwsh
```

```
> 1..5
1
2
3
4
5
> exit
```

動作する！。
















