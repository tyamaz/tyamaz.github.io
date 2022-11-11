---
title: Codepen_で_Hello_React_する
aliases:
  - Codepen_で_Hello_React_する
tags:
  - d/2022/09/20
  - n/PGM/React
  - n/PGM/HelloWorld
---

2022-09-20 現在の Codepen と React(おそらく ver 18.2.0 付近) の状況での React で HelloWorld する方法。

まずまっさらな pen を1個作る

ここによると [CDN リンク – React](https://ja.reactjs.org/docs/cdn-links.html)

```html
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
```

CDN はこれらになるようだ。


setting を開いて `JS` の項目を見る

![](f/Pasted%20image%2020220920090539.png)


`Add External Script/Pens` というところがあって、さらに検索窓もあってそこに例として React も書いてあるので、普通に `React` と検索する。

そうすると思いっきりさっきのやつが出てくるので、それらを選択する。

![](f/Pasted%20image%2020220920090814.png)

こんな感じで2つ設定できた。

![](f/Pasted%20image%2020220920090911.png)

そして、React は jsx という JavaScript に似た JavaScript でない言語で書かれるので、それを JS へコンパイルするために Babel を使うのでそれを有効化する。


![](f/Pasted%20image%2020220920091200.png)

思いっきり書いてあるので大丈夫だろう。

![](f/Pasted%20image%2020220920091255.png)

設定できたので サンプルのコードを書く

HTML に入れ込むポイントを作る

```html
<div id="root"></div>
```

ここへコンポーネントを作って入れ込む

```jsx
class Hello extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <div>Hello</div>
    );
  }
}

// ========================================

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<Hello />);
```

書いたら勝手に実行される


![](f/Pasted%20image%2020220920092548.png)
できた。
































