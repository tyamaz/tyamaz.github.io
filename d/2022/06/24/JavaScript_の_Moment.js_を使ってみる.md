---
title: JavaScript の Moment.js を使ってみる
aliases:
  - JavaScript_の_Moment.js_を使ってみる
tags:
  - d/2022/06/24
  - n/PGM/JavaScript/Ope/時刻操作
  - n/PGM/JavaScript/Node.js
  - n/PGM/JavaScript/Moment.js/v2.29.3
---

インストール
================================================================================

今回は node 環境でやる

適当に準備する

```console
$ npm init
```

install

```console
$ npm install moment
```

npm だと何も指定しなければモジュールはローカルにインストールされる

HelloWorld
================================================================================

```javascript
var moment = require('moment');
console.log(moment().format());
```

```console
$ node hoge.js
2022-06-24T17:50:29+07:00
```

こうらしい。簡単。


中を見る

```javascript
var moment = require('moment');
console.log(moment());
```


```javascript
Moment {  
    _isAMomentObject: true,  
    _isUTC: false,  
    _pf: {  
        empty: false,  
        unusedTokens: [],  
        unusedInput: [],  
        overflow: -2,  
        charsLeftOver: 0,  
        nullInput: false,  
        invalidEra: null,  
        invalidMonth: null,  
        invalidFormat: false,  
        userInvalidated: false,  
        iso: false,  
        parsedDateParts: [],  
        era: null,  
        meridiem: null,  
        rfc2822: false,  
        weekdayMismatch: false  
    },  
    _locale: Locale {  
        _calendar: {  
            sameDay: '[Today at] LT',  
            nextDay: '[Tomorrow at] LT',  
            nextWeek: 'dddd [at] LT',  
            lastDay: '[Yesterday at] LT',  
            lastWeek: '[Last] dddd [at] LT',  
            sameElse: 'L'  
        },  
        _longDateFormat: {  
            LTS: 'h:mm:ss A',  
            LT: 'h:mm A',  
            L: 'MM/DD/YYYY',  
            LL: 'MMMM D, YYYY',  
            LLL: 'MMMM D, YYYY h:mm A',  
            LLLL: 'dddd, MMMM D, YYYY h:mm A'  
        },  
        _invalidDate: 'Invalid date',  
        ordinal: [Function: ordinal],  
        _dayOfMonthOrdinalParse: /\d{1,2}(th|st|nd|rd)/,  
        _relativeTime: {  
            future: 'in %s',  
            past: '%s ago',  
            s: 'a few seconds',  
            ss: '%d seconds',  
            m: 'a minute',  
            mm: '%d minutes',  
            h: 'an hour',  
            hh: '%d hours',  
            d: 'a day',  
            dd: '%d days',  
            w: 'a week',  
            ww: '%d weeks',  
            M: 'a month',  
            MM: '%d months',  
            y: 'a year',  
            yy: '%d years'  
        },  
        _months: [  
            'January', 'February',  
            'March', 'April',  
            'May', 'June',  
            'July', 'August',  
            'September', 'October',  
            'November', 'December'  
        ],  
        _monthsShort: [  
            'Jan', 'Feb', 'Mar',  
            'Apr', 'May', 'Jun',  
            'Jul', 'Aug', 'Sep',  
            'Oct', 'Nov', 'Dec'  
        ],  
        _week: { dow: 0, doy: 6 },  
        _weekdays: [  
            'Sunday',  
            'Monday',  
            'Tuesday',  
            'Wednesday',  
            'Thursday',  
            'Friday',  
            'Saturday'  
        ],  
        _weekdaysMin: [  
            'Su', 'Mo',  
            'Tu', 'We',  
            'Th', 'Fr',  
            'Sa'  
        ],  
        _weekdaysShort: [  
            'Sun', 'Mon',  
            'Tue', 'Wed',  
            'Thu', 'Fri',  
            'Sat'  
        ],  
        _meridiemParse: /[ap]\.?m?\.?/i,  
        _eras: [ [Object], [Object] ],  
        _abbr: 'en',  
        _config: {  
            calendar: [Object],  
            longDateFormat: [Object],  
            invalidDate: 'Invalid date',  
            ordinal: [Function: ordinal],  
            dayOfMonthOrdinalParse: /\d{1,2}(th|st|nd|rd)/,  
            relativeTime: [Object],  
            months: [Array],  
            monthsShort: [Array],  
            week: [Object],  
            weekdays: [Array],  
            weekdaysMin: [Array],  
            weekdaysShort: [Array],  
            meridiemParse: /[ap]\.?m?\.?/i,  
            eras: [Array],  
            abbr: 'en'  
        },  
        _dayOfMonthOrdinalParseLenient: /\d{1,2}(th|st|nd|rd)|\d{1,2}/  
    },
    _d: 2022-06-24T10:52:16.010Z,  
    _isValid: true  
}
```

`moment` メソッドによって `Moment` オブジェクトが作られるようだ。

内部はこのような構造になっているっぽい。

内部にタイムゾーンとかオフセット情報を持ってないっぽいので、時刻の1点に対して表記変換を目的としているっぽい。
おそらく表記に対するタイムゾーン等の情報は都度与えるという思想なのかな？

Moment オブジェクトを作る
================================================================================
現在時刻で作る
--------------------------------------------------------------------------------
引数無しで `moment` メソッドを呼ぶと現在時で作られる


```javascript
const now = moment();
```




時刻っぽい文字列で作る 基本
--------------------------------------------------------------------------------

```javascript
const m = moment("2022-06-24T01:23:45");
console.log(m.format());  // 2022-06-24T01:23:45+07:00
```

デフォルトでその環境のオフセットとして解釈されて、出力もその環境でのモノがデフォルトになる。この形式は所謂 `ISO8601` 形式の一種で、

```javascript
Moment {  
    _isAMomentObject: true,  
    _i: '2022-06-24T01:23:45',  
    _f: 'YYYY-MM-DDTHH:mm:ss',  
    _isUTC: false,  
    _pf: {  
    empty: false,  
    unusedTokens: [],  
    unusedInput: [],  
    overflow: -1,  
    charsLeftOver: 0,  
    nullInput: false,  
    invalidEra: null,  
    invalidMonth: null,  
    invalidFormat: false,  
    userInvalidated: false,  
    iso: true,  
    parsedDateParts: [ 2022, 5, 24, 1, 23, 45 ],  
    era: null,  
    meridiem: undefined,  
    rfc2822: false,  
    weekdayMismatch: false
    ...
    _d: 2022-06-23T18:23:45.000Z,
    ...
},
```


内部もこうなっていて、オフセットやタイムゾーン情報は無いので、これは指定無しの場合はロジック側で持っていて、都度与えるという思想のようだ。


時刻っぽい文字列で作る オフセット指定
--------------------------------------------------------------------------------
ではオフセット指定で使ってみる

```javascript
const m = moment("2022-06-24T01:23:45+09:00");
console.log(m.format());  // 2022-06-23T23:23:45+07:00
```

日本時間で作ってみる。表示は実行環境ロケール(ベトナム時間)になっているので、表記上は2時間前になる。
つまり前日の `23時23分45秒` である。そのようだ。

内部を見ると今までに無かった `_tzm` という項目が出てきた。

```javascript
Moment {  
    _isAMomentObject: true,  
    _i: '2022-06-24T01:23:45+09:00',  
    _f: 'YYYY-MM-DDTHH:mm:ssZ',  
    _tzm: 540,  
    _isUTC: false,  
    _pf: {
```


これは明らかに `+9 x 60` なのでオフセットの分数を表している。
指定しなければ環境依存、指定すれば環境変わっても環境非依存という考えのようだ。


指定したフォーマットで作る
--------------------------------------------------------------------------------
用意される形式が ISO 形式とは限らない。

日本でよくあるモノだったら年月日のスラッシュ区切りである

```
2022/06/24
```

普通にこのようにやると

```javascript
const m = moment("2022/06/24");
```

↓のようなメッセージの例外が発生する

```
Deprecation warning: value provided is not in a recognized RFC2822 or ISO format.
```

そこで `moment` メソッドの第二引数にフォーマットを文字列で指定する

```javascript
const m = moment("2022/06/24", 'YYYY/MM/DD');
console.log(m.format()); //=> 2022-06-24T00:00:00+07:00
```

これでエラーもでず解釈もされることになる。
指定して無い部分(時分秒部分)は考えないのではなく、時刻が解釈オフセットでの `00:00:00` と見なされる


指定したフォーマットで作る 表記に無い情報を加える
--------------------------------------------------------------------------------

```javascript
const m = moment("2022/06/24", 'YYYY/MM/DD');
console.log(m.format()); //=> 2022-06-24T00:00:00+07:00
```

この例ではオフセット情報を与えていないので、その環境での情報で確定してしまう。
表示もその環境情報でやっているので一致していることがわかると思う。

フォーマットには無いのだが、日本時間(オフセット+9時間)で固定したい場合もあるだろう。それにはどうすればよいか。

Moment.js にはそのような解釈を変更するような操作は無いようだ。(あるかもしれんが・・・見た感じ無い)

なので単純に解釈を足してやればいい、表記に無い情報を別口で加えるのではなく、表記に単に加えればよい。

```javascript
const m = moment("2022/06/24" + "+09:00", 'YYYY/MM/DD' + "ZZ");
console.log(m.format()); //=> 2022-06-23T22:00:00+07:00
```

オフセットの表記は `ZZ` で解釈できるのでそれを加えてやればいいだけである。
日本での深夜0時解釈になるので、実行環境(`+07:00`)の解釈では前日の `22:00` になる。

正しい。



文字列で出力する
================================================================================
単に出力する
--------------------------------------------------------------------------------
引数無しで `format` メソッドを使うと、現在タイムゾーンでこのフォーマットで出る

```javascript
const m = moment("2022-06-24T01:23:45+09:00");
console.log(m.format());  // 2022-06-23T23:23:45+07:00
```


オフセットを指定して出力する
--------------------------------------------------------------------------------
時刻は1点を表しているので、それが状況によっては表記が変わってくる

オフセット指定で出力してみる

```javascript
const m = moment("2022-06-24T01:23:45+09:00");  // 日本時間指定で作る
m.utcOffset("+09:00");  // 表示を日本時間指定にする(指定しなければ実行環境依存)
console.log(m.format()); //=> 2022-06-24T01:23:45+09:00
console.log(m.format()); //=> 2022-06-24T01:23:45+09:00
```

`utcOffset` メソッドを使う。

内部状態を set してしまっているので、何度やろうが、指定したオフセット表記になる。

途中で変えることもできる。

```javascript
const m = moment("2022-06-24T01:23:45+09:00");
m.utcOffset("+09:00");
console.log(m.format()); //=> 2022-06-24T01:23:45+09:00
m.utcOffset("+08:00");
console.log(m.format()); //=> 2022-06-24T00:23:45+09:00
```


内部を見ると

```javascript
Moment {  
    _isAMomentObject: true,  
    _i: '2022-06-24T01:23:45+09:00',  
    _f: 'YYYY-MM-DDTHH:mm:ssZ',  
    _tzm: 540,  
    _isUTC: true,  
    _pf: {},
    ...
    _d: 2022-06-24T00:23:45.000Z,  
    _isValid: true,  
    _offset: 480  
}
```


`_offset` というオフセット情報が付加されていてこいつに基づいて出力が決定されるようだ。
`_d` の値はこのオフセット情報に基づいて確定した値に見えるので、計算量削減のために毎度計算せずにメモ化してるのだろう。

作成時に指定した `_tzm` は入力時の情報を維持していて、入力情報 + 入力時オフセット → UTC + 出力オフセット → 出力 となっているようだ。


この `utsOffset` メソッドの戻りは自身の `Moment` オブジェクトそのものなのでメソッドチェーンで記述できる。

```javascript
const m = moment("2022-06-24T01:23:45+09:00");
console.log(m.utcOffset("+09:00").format()); //=> 2022-06-24T01:23:45+09:00
```


指定フォーマットで出力する
--------------------------------------------------------------------------------
何も指定しなければ `2022-06-24T01:23:45+09:00` のような感じになり、これは所謂 `ISO8601` 形式の一種。

`format` メソッドに形式を文字列で指定するとその形式で吐き出してくれる。

アメリカっぽく吐き出したならこうなる

```javascript
const m = moment("2022-06-24T01:23:45+09:00");
m.utcOffset("+09:00");
console.log(m.format());   //=> 2022-06-24T01:23:45+09:00 
console.log(m.format("dddd, MMMM Do YYYY, h:mm:ss a"));
//=> Friday, June 24th 2022, 1:23:45 am
```
 
このへんの形式は公式マニュアルを読めばよい

- [Format \- momentjs\.com](https://momentjscom.readthedocs.io/en/latest/moment/04-displaying/01-format/)




















