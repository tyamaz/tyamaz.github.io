---
title: Google Chrome / Bookmarklet を作る
aliases:
  - Google Chrome / Bookmarklet を作る
  - ブックマークレットを作る
tags: [ブックマークレット]
---

ブラウザの URL 欄に JavaScript を貼り付けたら、あたかもその画面でそもそも埋まってたかのようにその JS コードが駆動できるアレを作る。

実行したい JavaScript を書く
================================================================================
まずは実行したい JavaScript コードを書く。

今回は、表示しているページを markdown のリンク形式でクリップボードに入れるというよくありげなものを作る。

拡張として山のように実装されているが、こういう誰でも作れる、山のように存在する、誰が作ったなんか誰も気にしないような拡張は作者がメンテ放棄したりして、その後乗っ取られてヤバいコードが仕込まれたりする可能性が高いのでこのぐらいなら自分で実装して安心したいし、ブックマークレットで実装しておくと `Vimium` との連携が楽にできるというのがある。

コードはこうなる

```javascript
    let url = location.href;
    url = url.split("(").join("%28");
    url = url.split(")").join("%29");
    let title = document.querySelector("title").innerText.split("\n").join(" ").trim();
    title = title.split("\\").join("\\\\");
    title = title.split("`").join("\\`");
    title = title.split("*").join("\\*");
    title = title.split("_").join("\\_");
    title = title.split("[").join("\\[");
    title = title.split("]").join("\\]");
    title = title.split("{").join("\\{");
    title = title.split("}").join("\\}");
    title = title.split("(").join("\\(");
    title = title.split(")").join("\\)");
    title = title.split("+").join("\\+");
    title = title.split("-").join("\\-");
    title = title.split(".").join("\\.");
    title = title.split("!").join("\\!");
    title = title.split("#").join("\\#");
    const mdLink='[' + title + '](' + url + ')';
    navigator.clipboard.writeText(mdLink);
```

ものすごい簡単なコードである。

このへん [Daring Fireball: Markdown Syntax Documentation](https://daringfireball.net/projects/markdown/syntax#backslash) に基づいて title 部分をエスケープする

URL 部分は、括弧部分だけ markdown にひっかかるのでそこを urlencode する。あまりなさそうに思えるが Wikipedia のリンクで高頻度で登場するので必要になる。

最後にできあがった markdown をクリップボードに突っ込む


Bookmarklet化する
================================================================================
URL欄に突っ込むという性質上、JS なんでも入れられるわけでなく、特殊な形式にしなければならない。

簡単に言うと、いろんな記号をエスケープして1行で書く必要がある。
結構面倒なので、[Bookmarkleter](https://chriszarate.github.io/bookmarkleter/) このような変換ツールがあるのでつっこめばそれをやってくれる。

↑をやったものが

```
javascript:void%20function(){let%20a=location.href;a=a.split(%22(%22).join(%22%2528%22),a=a.split(%22)%22).join(%22%2529%22);let%20b=document.querySelector(%22title%22).innerText.split(%22\n%22).join(%22%20%22).trim();b=b.split(%22\\%22).join(%22\\\\%22),b=b.split(%22`%22).join(%22\\`%22),b=b.split(%22*%22).join(%22\\*%22),b=b.split(%22_%22).join(%22\\_%22),b=b.split(%22[%22).join(%22\\[%22),b=b.split(%22]%22).join(%22\\]%22),b=b.split(%22{%22).join(%22\\{%22),b=b.split(%22}%22).join(%22\\}%22),b=b.split(%22(%22).join(%22\\(%22),b=b.split(%22)%22).join(%22\\)%22),b=b.split(%22+%22).join(%22\\+%22),b=b.split(%22-%22).join(%22\\-%22),b=b.split(%22.%22).join(%22\\.%22),b=b.split(%22!%22).join(%22\\!%22),b=b.split(%22%23%22).join(%22\\%23%22);const%20c=%22[%22+b+%22](%22+a+%22)%22;navigator.clipboard.writeText(c)}();
```



使う
================================================================================
できあがったコードをブックマークとして登録し、対象のページが表示されている時に呼び出せば実行される。





















