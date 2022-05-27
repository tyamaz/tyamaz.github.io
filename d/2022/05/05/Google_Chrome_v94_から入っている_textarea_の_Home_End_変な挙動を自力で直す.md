---
title: Google_Chrome_v94_から入っている_textarea_の_Home_End_変な挙動を自力で直す
aliases:
  - Google_Chrome_v94_から入っている_textarea_の_Home_End_変な挙動を自力で直す
tags:
  - d/2022/05/05
---

Chrome が v100 も超えてもまったく直す気も無いようなので適当に自力で直す。

- Windows10
- Google Chrome 100.0.4896.127
- Tampermonkey v4.16


問題。
`textarea` 内で `Home` キーを押した場合にカーソルが行頭に無いなら行頭に移動する。
行頭にあるなら何もおきない。しかし空行の行頭にある場合は最上行へジャンプするという誰が嬉しいのかよくわからない挙動をする。他のエディタやブラウザではこんな変な動きは一切しないので、Chrome の中の人が明らかにおかしい。こいつを抑制する。


行頭へのジャンプはどのタイミングで行われるのか？
================================================================================
調べると、`Home` キーの `keydown` イベントで行われる。従って `keyup` や `keypress` に仕込んでもダメ、遅い。

```javascript
ta.addEventListener("keydown", function(event){
});
```

Home キーの検知はどうするのか？
================================================================================
発生イベントから `code` プロパティで取り文字列として比較する。
キー単体へのイベントフックというものは無い。

```javascript
if(event.code == "Home"){
```

最上行へのジャンプをどう抑制する？
================================================================================
ブラウザの標準挙動の抑制は `event.preventDefault()` なのでこれで抑制できた。


カーソルの行頭判定はどうする？
================================================================================
`ta.value` から中身全部を得られる。`ta.selectionStart` で現在のカーソル位置がわかる。

```javascript
ta.value.substr(ta.selectionStart - 1, 1) == "\n"
```

カーソル位置の1個前から1文字分が改行ならばカーソル位置は行頭ということになる



スクリプト作成
================================================================================
全部の部品が揃ったのであとは組み合わせるだけ。


```javascript
// ==UserScript==
// @name         Fix textarea home end behavior
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  unko
// @author       hoge
// @match        https://example.com
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        none
// ==/UserScript==

(function() {
    'use strict';
    const taList = document.querySelectorAll("textarea");
    for(const ta of taList){
        ta.isCursorHome = function(){
            if(this.selectionStart == 0){
                return true;
            }
            if(this.value.substr(this.selectionStart - 1, 1) == "\n"){
                return true;
            }
            return false;
        };
        ta.addEventListener("keydown", function(event){
            if(event.code == "Home"){
                if(ta.isCursorHome()){
                    event.preventDefault();
                    return;
                }
            }
        });

    }
})();
```


これで問題無い。`End` も微妙に変な動きをするのだが、実害が出るほどでないのでほっとく。
問題なのは空行の `Home` なのだが切り分けなくても動作に問題ないので空行判定はしてない。














