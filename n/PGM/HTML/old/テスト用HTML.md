---
title: HTML / テスト用HTML
aliases:
  - HTML / テスト用HTML
tags:
  - d/2009/03/12
  - old
---

2009年03月12日 記述。使えなくも無いがやや古いと思ったほうがよい

何かテストするときにいつもなんかコピペしてるので、それ用

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta name="Content-Type" content="text/html;charset=UTF-8" />
    <title>プレーンなタグ集</title>
    <meta http-equiv="Content-Style-Type" content="text/css" />
    <meta http-equiv="Content-Script-Type" content="text/javascript" />
    <link rel="stylesheet" href="hoge.css" type="text/css" />
    <script type="text/javascript" src="hoge.js" charset="UTF-8"></script>
</head>
<body>
    <hr />
    <h2>見出し(h2)</h2>
    <h1>h1タグです</h1>
    <h2>h2タグです</h2>
    <h3>h3タグです</h3>
    <h4>h4タグです</h4>
    <h5>h5タグです</h5>
    <h6>h6タグです</h6>
    <hr />

    <h2>inline部品(h2)</h2>
    <p>pタグです。段落に使います。</p>
    <p>ここはpタグです→<a href="http://www.yahoo.co.jp/">aタグです</a>yahooに飛びます。</p>
    <p>ここはpタグです</p>


    <hr />

    <h2>箇条書き(h2)</h2>
    <h3>単なる箇条書き(h3)</h3>
    <ul>
        <li>リスト1</li>
        <li>リスト2</li>
        <li>リスト3</li>
    </ul>
    
    <h3>単なる順序付箇条書き(h3)</h3>
    <ol>
        <li>リスト1</li>
        <li>リスト2</li>
        <li>リスト3</li>
    </ol>

    <h3>単なる入れ子箇条書き(h3)</h3>
    <ul>
        <li>
            リスト1
            <ul>
                <li>リスト1-1</li>
                <li>リスト1-2</li>
                <li>リスト1-3</li>
                <li>
                    リスト1-3
                    <ul>
                        <li>リスト1-3-1</li>
                        <li>リスト1-3-2</li>
                        <li>リスト1-3-3</li>
                    </ul>
                </li>
            </ul>
        </li>
        <li>
            <ul>
                <li>リスト2-1(階層名無し)</li>
                <li>リスト2-2(階層名無し)</li>
                <li>リスト2-3(階層名無し)</li>
            </ul>
        </li>
        <li>リスト3</li>
    </ul>
    <hr />

    <h2>UI(h2)</h2>
    <input type="text" value="inputです" />
    <fieldset>
        <legend>ここlegend</legend>
        <label>ラベル</label>
        <input type="text" value="inputです" />  
    </fieldset>
    <fieldset>
        <legend>radio name 被り版</legend>
        <label>radio1</label>
        <input type="radio" value="1" name="radioname" />
        <label>radio2</label>
        <input type="radio" value="2" name="radioname" />
        <label>radio3</label>
        <input type="radio" value="3" name="radioname" />
    </fieldset>
</body>
</html>
```


