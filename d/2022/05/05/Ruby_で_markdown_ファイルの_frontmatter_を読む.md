---
title: Ruby_で_markdown_ファイルの_frontmatter_を読む
aliases:
  - Ruby_で_markdown_ファイルの_frontmatter_を読む
tags:
  - d/2022/05/05
---

アプローチ1 frontmatter として読む
================================================================================
[GitHub \- waiting\-for\-dev/front\_matter\_parser: Ruby library to parse files or strings with a front matter\. It has automatic syntax detection\.](https://github.com/waiting-for-dev/front_matter_parser)

それっぽいやつがあったので

```
> bundle init
```

`Gemfile` が生成されるのでそこに

```ruby
gem 'front_matter_parser'
```

を書き込む。

```console
> bundle
```

準備OK。Windows 環境なのでこのへんは雑にやる。


コードを書く

```ruby
require "front_matter_parser"

parsed = FrontMatterParser::Parser.parse_file('hogehoge.md')
p parsed.front_matter["tags"][0]
p parsed.content
```


対象の markdown はこれ

```markdown
---
title: Ruby_で_markdown_ファイルの_frontmatter_を読む
aliases:
  - Ruby_で_markdown_ファイルの_frontmatter_を読む
tags:
  - d/2022/05/05
---


なんかする
```


結果

```
> ruby hogehoge.rb
"d/2022/05/05"  
"なんかする\n"
```

OK


frontmatter がない場合は結果こうなる

```
>ruby hogehoge.rb  
{}  
"\nなんかする\n"
```

空の Hash が返ってくる。


アプローチ2 先頭部分を切り出して単なる YAML として読む
================================================================================
あまりにあっさり出来てしまったのでヤル必要がまったくなくなってしまった。











