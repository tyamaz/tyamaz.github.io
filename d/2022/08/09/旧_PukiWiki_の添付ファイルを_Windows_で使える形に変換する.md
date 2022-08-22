---
title: 旧 PukiWiki の添付ファイルを Windows で使える形に変換する
aliases:
  - 旧_PukiWiki_の添付ファイルを_Windows_で使える形に変換する
tags:
  - d/2022/08/09
  - n/PGM/Ruby/etc
  - n/PGM/Ruby/Ope/ファイル操作
  - n/PGM/PukiWiki
---

旧 PukiWiki のページへの添付ファイル(attach ディレクトリの中)を Windows で使える形に変換する。今頃。

ファイル仕様
================================================================================
ファイル自体はアップされたバイナリそのまま。ファイル名に特徴。

添付されたページ名の EUCJP エンコードバイナリの 16進 表現、アンスコの添付ファイルのオリジナル名の EUCJP エンコードバイナリの 16進 表現。

拡張子が無いやつがオリジナルファイルになる。

```
4D7953514C2FA5A4A5F3A5B9A5C8A1BCA5EBB4B0C1B4BCEABDE7BDF1A1CA57696E646F7773C8C7A1CB_696D61676532303038303430383139353632312E6A7067
```

↑のデータを戻す

```
MySQL/インストール完全手順書（Windows版）
image20080408195621.jpg
```

という形になる。

実装
================================================================================
Ruby で実装。

- Windows10
- Ruby 3.1.2

雑

```ruby
require 'fileutils'
Dir.glob("./attach/*").each  do | ori |

  next unless File.file?(ori)
  name = File.basename(ori)
  next unless File.extname(name) == ""
  puts name
  d = name.split("_")[0]
  f = name.split("_")[1]

  d = [d].pack("H*").encode('UTF-8', 'euc-jp')
  f = [f].pack("H*").encode('UTF-8', 'euc-jp')

  FileUtils.mkdir_p("./out/" + d)
  FileUtils.cp(ori, "./out/" + d + "/" + f)
end

```













