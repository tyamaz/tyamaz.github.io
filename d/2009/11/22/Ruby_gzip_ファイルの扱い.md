---
title: Ruby gzip ファイルの扱い
aliases:
  - Ruby_gzip_ファイルの扱い
tags:
  - d/2009/11/22
  - n/PGM/Ruby/Ope/ファイル操作
  - n/PGM/Ruby/v1.8.6
---

- 2009-11-22
- Ruby 1.8.6

単一ファイルを展開する
================================================================================
べたべたに展開してみる

```ruby
require 'zlib'
file_name = "hoge.gz"
gz = Zlib::GzipReader.open(file_name)
if gz.eof?
  gz.close
  exit
end
        
piyo = File.open("piyo.txt", "w+b")
gz.each do |line|          
  piyo.puts(line)
end
piyo.close
gz.close
```

単一ファイルを圧縮

```ruby
Zlib::GzipWriter.open("hoge.gz", Zlib::BEST_COMPRESSION) do |gz|
  gz.mtime = File.mtime(file_path) 
  gz.orig_name = File::basename("hoge.txt")
  f = File.open("hoge.txt", 'rb')
  f.each do |line|
    gz.puts line
  end
  f.close
end
```

