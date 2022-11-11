---
title: React で親コンポーネントから子を参照する
aliases:
  - React_で親コンポーネントから子を参照する
tags:
  - d/2022/09/19
  - n/PGM/React/v18.2
---

したかったので。

自身が render で生成するコンポーネントを使うには ref 属性を記述する。
参照を先に作って、後から参照先を入れ替えるみたいなことをやるが、あまり考えなくていい。

例のためのコードになってしまいややごちゃついているが簡単である。

```jsx
class Ko extends React.Component{
  constructor(props){
    super(props);
    this.mySelect = React.createRef();
    this.onChangeOya = props.onChange;
  }
  changeHandler(e){
    const v = this.mySelect.current.value;
    console.log("ko " + v);
    this.onChangeOya();
  }
  getV(){
    return this.mySelect.current.value;
  }
  render(){
    return (
      <select ref={ this.mySelect } onChange={ (e)=>{ this.changeHandler(); } }>
        <option value={ 1 }>hoge</option>
        <option value={ 2 }>piyo</option>
        <option value={ 3 }>fuga</option>
      </select>
    );
  }
}
class Oya extends React.Component {
  constructor(props) {
    super(props);
    this.ko = React.createRef();
  }
  handleChange(v) {
    console.log("oya " + this.ko.current.getV());
  }
  render() {
    return (
      <Ko ref={ this.ko } onChange={ (v)=>{ this.handleChange(v); } } />
    );
  }
}

// ========================================

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<Oya />);
```

`React.createRef` で参照を振りだして、それをコンポーネントの `ref` 属性に付与すれば後でその参照を通じて `current` で取得できる。

設計としては、1個下の参照ぐらいは親は掴んでおくべきだと思う。







