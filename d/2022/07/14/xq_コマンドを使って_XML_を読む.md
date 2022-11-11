---
title: xq コマンドを使って XML を読む
aliases:
  - yq_コマンドを使って_XML_を読む
tags:
  - d/2022/07/14
  - n/PGM/XML
  - n/PGM/Linux/コマンド
  - n/PGM/Linux/d/Linux_Mint/v20.1
---

YAML を操作するための `jq` コマンドという位置づけの `yq` というコマンドがあり、そのオマケで XML も操作できるような`xq` コマンドがあるようなのでやってみる。

Install
================================================================================
このコマンドは実装としては `jq` のラッパーなのでそれはインストールしてある必要がある。

```
$ sudo apt install jq
```



`pip` でインストールする

```
$ pip install yq
$ yq --version
yq 3.0.2
```

`xq` コマンドもインストールされる

```
$ xq --version
xq 3.0.2
```


以上

使ってみる
================================================================================
適当な XML として VOA のポッドキャスト https://learningenglish.voanews.com/podcast/?zoneId=3521 フィードを取ってきてみる。

```
$ curl "https://learningenglish.voanews.com/podcast/?zoneId=3521"
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">     
  <channel>      
    <title>As It Is - VOA Learning English</title>     
    <link>https://learningenglish.voanews.com/z/3521</link>
    <itunes:image href="https://gdb.voanews.com/DACF054E-5055-4CFB-B4AE-34DAE504BF5D.jpg" />
    <itunes:summary>As It Is takes a daily look at issues in the news in the United States and around the world.</itunes:summary>
    <description>As It Is takes a daily look at issues in the news in the United States and around the world.</description>
    <itunes:explicit>no</itunes:explicit>
    <language>en</language>
    <copyright>2022 - VOA</copyright>   
    <ttl>60</ttl>        
    <lastBuildDate>Thu, 14 Jul 2022 10:35:32 +0000</lastBuildDate> 
    <generator>Pangea CMS – VOA</generator>        
    <itunes:author>VOA Learning English</itunes:author>
    <itunes:owner>
      <itunes:name>VOA Learning English</itunes:name>
      <itunes:email>voalearningenglish@gmail.com</itunes:email>
    </itunes:owner>
    <itunes:category text="Education">
      <itunes:category text="Language Learning"/>
    </itunes:category>
    <atom:link href="https://learningenglish.voanews.com/podcast/?zoneId=3521" rel="self" type="application/rss+xml" />
    <item>
      <title>Sri Lankan President Flees, Protesters Storm PM’s Office - July 13, 2022</title>
      <description></description>
      <guid>https://learningenglish.voanews.com/a/6657532.html</guid>            
      <pubDate>Wed, 13 Jul 2022 19:56:14 +0000</pubDate>      
      <itunes:author>VOA Learning English</itunes:author>
      <itunes:summary></itunes:summary>
      <itunes:duration>00:05:56</itunes:duration>                
      <itunes:image href="https://gdb.voanews.com/6AD44B97-4251-4D10-B871-C134BBFB39FA_w640_h360.jpg" /> 
      <enclosure url="https://av.voanews.com/clips/VLE/2022/07/13/09680000-0a00-0242-240f-08da6509be25_hq.mp3" type="audio/mpeg" length="5832704" />
    </item>		
    <item>
      <title>US Promises Increased Aid to Pacific Island Nations - July 13, 2022</title>
      <description></description>
      <guid>https://learningenglish.voanews.com/a/6657482.html</guid>            
      <pubDate>Wed, 13 Jul 2022 19:31:13 +0000</pubDate>      
      <itunes:author>VOA Learning English</itunes:author>
      <itunes:summary></itunes:summary>
      <itunes:duration>00:05:50</itunes:duration>                
      <itunes:image href="https://gdb.voanews.com/6AD44B97-4251-4D10-B871-C134BBFB39FA_w640_h360.jpg" /> 
      <enclosure url="https://av.voanews.com/clips/VLE/2022/07/13/09690000-0a00-0242-2228-08da65063f11_hq.mp3" type="audio/mpeg" length="5734400" />
    </item>
    ...
```

こんな感じのやつを


こいつを `xq` に食わせる。とりあえず全件吐き出しにする

```
$ curl "https://learningenglish.voanews.com/podcast/?zoneId=3521" | xq "."
```


こう吐き出される。

```json
{
  "rss": {
    "@xmlns:itunes": "http://www.itunes.com/dtds/podcast-1.0.dtd",
    "@version": "2.0",
    "@xmlns:atom": "http://www.w3.org/2005/Atom",
    "channel": {
      "title": "As It Is - VOA Learning English",
      "link": "https://learningenglish.voanews.com/z/3521",
      "itunes:image": {
        "@href": "https://gdb.voanews.com/DACF054E-5055-4CFB-B4AE-34DAE504BF5D.jpg"
      },
      "itunes:summary": "As It Is takes a daily look at issues in the news in the United States and around the world.",
      "description": "As It Is takes a daily look at issues in the news in the United States and around the world.",
      "itunes:explicit": "no",
      "language": "en",
      "copyright": "2022 - VOA",
      "ttl": "60",
      "lastBuildDate": "Thu, 14 Jul 2022 10:35:32 +0000",
      "generator": "Pangea CMS – VOA",
      "itunes:author": "VOA Learning English",
      "itunes:owner": {
        "itunes:name": "VOA Learning English",
        "itunes:email": "voalearningenglish@gmail.com"
      },
      "itunes:category": {
        "@text": "Education",
        "itunes:category": {
          "@text": "Language Learning"
        }
      },
      "atom:link": {
        "@href": "https://learningenglish.voanews.com/podcast/?zoneId=3521",
        "@rel": "self",
        "@type": "application/rss+xml"
      },
      "item": [
        {
          "title": "Sri Lankan President Flees, Protesters Storm PM’s Office - July 13, 2022",
          "description": null,
          "guid": "https://learningenglish.voanews.com/a/6657532.html",
          "pubDate": "Wed, 13 Jul 2022 19:56:14 +0000",
          "itunes:author": "VOA Learning English",
          "itunes:summary": null,
          "itunes:duration": "00:05:56",
          "itunes:image": {
            "@href": "https://gdb.voanews.com/6AD44B97-4251-4D10-B871-C134BBFB39FA_w640_h360.jpg"
          },
          "enclosure": {
            "@url": "https://av.voanews.com/clips/VLE/2022/07/13/09680000-0a00-0242-240f-08da6509be25_hq.mp3",
            "@type": "audio/mpeg",
            "@length": "5832704"
          }
        },
        {
          "title": "US Promises Increased Aid to Pacific Island Nations - July 13, 2022",
          "description": null,
          "guid": "https://learningenglish.voanews.com/a/6657482.html",
          "pubDate": "Wed, 13 Jul 2022 19:31:13 +0000",
          "itunes:author": "VOA Learning English",
          "itunes:summary": null,
          "itunes:duration": "00:05:50",
          "itunes:image": {
            "@href": "https://gdb.voanews.com/6AD44B97-4251-4D10-B871-C134BBFB39FA_w640_h360.jpg"
          },
          "enclosure": {
            "@url": "https://av.voanews.com/clips/VLE/2022/07/13/09690000-0a00-0242-2228-08da65063f11_hq.mp3",
            "@type": "audio/mpeg",
            "@length": "5734400"
          }
        },
```

XML が JSON に変換されてしまうのだ。つまりこの後は操作としては `jq` そのものということになる。
属性は `@` から始まるキーに割り当てられてタグはそのままのキーで割り当て荒れるようだ。タグ有りの中身無しは `null` にマップされる。配下の複数の同一タグは1つの配列にまとめられてそのタグ名がキーになる

これを考慮すれば url の一覧を得るのも簡単だ。


```
$ curl "https://learningenglish.voanews.com/podcast/?zoneId=3521" | xq -r ".rss.channel.item[].enclosure[\"@url\"]"
```

これで全 URL の一覧が手に入る。ポイントは `@` アットマーク 等の特殊記号が入ったキーでこの場合 jq ではダブルクォートで括ってやる必要が出てくる、シェルとしてもダブルクォートは意味あるので、バックスラッシュでエスケープしている。






