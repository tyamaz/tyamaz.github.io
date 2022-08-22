---
title: React HelloWorld
aliases:
  - React_HelloWorld
tags:
  - d/2022/07/11
---

環境
================================================================================

- 概ね 2022-07-11 の環境
- Linux Mint 20.1
- Node.js 18.0.0


Node の確認
================================================================================
以前何かで入れたやつがあったのでそれをそのまま使う。確認。

```console
$ node --version
v18.0.0
```


React のプロジェクトを作る
================================================================================
適当なディレクトリで↓のコマンドを打つ。

```console
$ npx create-react-app hoge_app

Need to install the following packages:
  create-react-app
Ok to proceed? (y) y
npm WARN deprecated tar@2.2.2: This version of tar is no longer supported, and will not receive security updates. Please upgrade asap.

Creating a new React app in /home/hoge/Documents/source/react/hoge_app.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template...


added 1393 packages in 50s

203 packages are looking for funding
  run `npm fund` for details

Initialized a git repository.

Installing template dependencies using npm...
npm WARN deprecated source-map-resolve@0.6.0: See https://github.com/lydell/source-map-resolve#deprecated

added 52 packages in 5s

203 packages are looking for funding
  run `npm fund` for details
Removing template package using npm...


removed 1 package, and audited 1445 packages in 3s

203 packages are looking for funding
  run `npm fund` for details

6 high severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
Git commit not created Error: Command failed: git commit -m "Initialize project using Create React App"
    at checkExecSyncError (node:child_process:817:11)
    at execSync (node:child_process:888:15)
    at tryGitCommit (/home/hoge/Documents/source/react/hoge_app/node_modules/react-scripts/scripts/init.js:62:5)
    at module.exports (/home/hoge/Documents/source/react/hoge_app/node_modules/react-scripts/scripts/init.js:350:25)
    at [eval]:3:14
    at Script.runInThisContext (node:vm:129:12)
    at Object.runInThisContext (node:vm:305:38)
    at node:internal/process/execution:76:19
    at [eval]-wrapper:6:22 {
  status: 128,
  signal: null,
  output: [ null, null, null ],
  pid: 15229,
  stdout: null,
  stderr: null
}
Removing .git directory...

Success! Created hoge_app at /home/hoge/Documents/source/react/hoge_app
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd hoge_app
  npm start

Happy hacking!
npm notice 
npm notice New minor version of npm available! 8.6.0 -> 8.13.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v8.13.2
npm notice Run npm install -g npm@8.13.2 to update!
npm notice 
```


いくつかの警告が出て、`git commit` ができなかったみたいな話が書いてあるが、特に問題ないだろう。


結果こうなる

```
$ tree -a -I "node_modules"
.
├── .gitignore
├── README.md
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    ├── reportWebVitals.js
    └── setupTests.js
```

メッセージの最後にこうしろとあるのでやる。

```
  cd hoge_app
  npm start
```


```
$ npm start
```

![](Pasted%20image%2020220711101449.png)

開発用サーバが動いて、ブラウザが勝手に立ち上がって `http://localhost:3000/` にアクセスして↑が表示される。
OK

説明として `src/App.js` を書き換えればよいようだ。


チュートリアル始め
================================================================================
[Hello World – React](https://ja.reactjs.org/docs/hello-world.html)

このドキュメントを読んでいく

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<h1>Hello, world!</h1>);
```

このように書くようだ。どこに書くとか書いてないが・・・


現状の `src/App.js` を読んでみる


```jsx
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}
export default App;
```

さっき表示されているやつはこれに見える。

では書き換えてみる。

```jsx
function App() {
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<h1>Hello, world!</h1>);
}

export default App;

```

そうすると立ち上げている開発サーバから怒られる

```
Failed to compile.

[eslint] 
src/App.js
  Line 2:16:  'ReactDOM' is not defined  no-undef

```

`ReactDOM` が無いと

何も考えずに Google に聞く。一発で出てくる。

```jsx
import ReactDOM from "react-dom";
```

これを足せと。

問題無い。

```
Compiled successfully!
```

ブラウザをリロードすると出る

![](Pasted%20image%2020220711102645.png)


OK

やや気になるとして `document.getElementById('root')` これ何処の何だよ？

試しに消してみる

```jsx
  const root = ReactDOM.createRoot();
  root.render(<h1>Hello, world!</h1>);
```

そうすると画面に何も出なくなった。なんだかこの `root` というのは何かの予約要素のようだ。



JSX とは
================================================================================
この部分に注目

```jsx
  root.render(<h1>Hello, world!</h1>);
```

JS の中に HTML っぽい何かが直書きされている。これが `React 要素` リテラルで、この `React 要素` を直書きで生成できる機構を組み込んだ JS とはやや違うこの言語を `JSX` と呼ぶ。

この HTML のタグっぽいモノで始まっていたらそれが終わるまでがリテラルと認識されるようだ。


React 要素リテラルでの値の展開
================================================================================
このように中括弧(波括弧)で変数をその場に展開できる。

```jsx
  const root = ReactDOM.createRoot(document.getElementById('root'));
  const name = "react";
  root.render(<h1>Hello, { name }!</h1>);
```

なのでこの結果は `Hello, react!` になる。

ではその波括弧はどう書けばいいのだというと、エスケープの記法は無いので、リテラルで埋め込めということのようだ


```jsx
  const root = ReactDOM.createRoot(document.getElementById('root'));
  const name = "react";
  root.render(<h1>Hello, { "{" }name{ "}" }!</h1>);
```

不格好だがこうなる。結果は `Hello, {name}!` こうなる

ということで、括弧の中は上からの地続きの JS 空間だということはわかった。



React 要素リテラルでの HTML 属性への値指定
================================================================================
React要素は HTML DOM ではない、そのリテラルも単なるテンプレートエンジンではない。

なので HTML への属性指定をするかのような記述はこうなる。

```jsx
root.render(<h1 className={name}>Hello, World!</h1>);
```

そうなると結果このようになる。

```html
<h1 class="react">Hello, World!</h1>
```

指定する属性名が微妙に違ったり、ダブルクォートで囲む必要が無かったりする。

こう考えると、オリジナルの属性を展開したりできない

```jsx
root.render(<h1 {name}={name}>Hello, World!</h1>);
```

これはエラーになる。属性を変数で任意の値に展開したりできない

決め打ちで独自要素は指定できるようだ

```jsx
root.render(<h1 hogePiyo={name}>Hello, World!</h1>);
```

今属性名をキャメルで記述したが HTML 上は

```html
<h1 hogepiyo="react">Hello, World!</h1>
```

このように、全て小文字で描画される。



React 要素の入れ子
================================================================================
これもできる

```jsx
  const name = "react";
  const ko = (<span>{ name }</span>);
  const oya = (<h1>{ ko }</h1>);
  root.render(oya);
```

これはこのように展開される。

```html
<h1><span>react</span></h1>
```


React要素で使う変数の確定タイミング
================================================================================
変数が確定するタイミングを見てみる。このようにする。

```jsx
  let name = "react1";
  const ko = (<span>{ name }</span>);
  name = "react2";
  const oya = (<h1>oya { name } ko { ko }</h1>);
  root.render(oya);
```

同じ変数を途中で値を変えてみる。

結果こうなる

```html
<h1>oya react2 ko <span>react1</span></h1>
```

つまりリテラルが評価された時点の値で確定されるということになる。
これは単純なものを作るのには優れるが、やや込み入ったモノを作るにはどうすればよいのだろう？

作る時点で値が必要になるという点もダイナミックな思考が必要でややよくない気がする。

参照も一応確認してみる


```jsx
const hoge = {
  name: function(){ return "hogehoge";}
};
const ko = (<span>{ hoge.name() }</span>);
hoge.name = function(){ return "piyopiyo";};
const oya = (<h1>oya { hoge.name() } ko { ko }</h1>);
root.render(oya);
```

よりダイナミックにしてみたが、結果は評価時で確定である。


```html
<h1>oya piyopiyo ko <span>hogehoge</span></h1>
```



React要素を HTMLっぽい文字列で作れるか？
================================================================================
こんな感じに

```jsx
  const span = "<span>unko</span>";
  const h1 = (<h1>{ span }</h1>);
  root.render(h1);
```

結果このように描画される。

```html
<h1>&lt;span&gt;unko&lt;/span&gt;</h1>
```

エスケープされてしまって自在に作ることができない。

どうしても作りたい場合は、このように書くようだ。しかしこの機能の名前からして使うなという意味だろう。

```jsx
  const span = "<span>unko</span>";
  const h1 = (<h1 dangerouslySetInnerHTML={{
      __html: span
    }}></h1>);
  root.render(h1);
```




j



ーーーー











