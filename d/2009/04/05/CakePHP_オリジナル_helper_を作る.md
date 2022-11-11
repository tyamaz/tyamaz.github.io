---
title: CakePHP オリジナル helper を作る
aliases:
  - CakePHP_オリジナルhelperを作る
tags:
  - d/2009/04/05
  - n/PGM/PHP/v5.2
  - n/PGM/PHP/CakePHP/v1.2/helper
---


- 2009年04月05日
- CakePHP1.2.8
- PHP5.2.6

オリジナルのhelperを作る
================================================================================
今回は CSS を自動読み込みする helper を作ります。
自分のCSSの設計方針は→[[CSS/設計]]なので、Cakeのレイアウト上の仕組みとはちょっと合わないのでこれのための helper を作る。

使用としては、controller名_action名　な感じの CSS ファイルを呼び出すだけ
あとは既存の

```php
$html->css
```

helperに任せる


ファイルの用意
================================================================================
`app/views/helpers` 以下にそのものズバリのファイル名で作る今回はAutoCssHelperみたいな名前にするので、ファイル名は`auto_css.php` です。


helperクラスを作る
================================================================================
とりあえず、`AppHelper` クラスを継承してあとはパスカル表記のクラス名で

```php
class AutoCssHelper exptends AppHelper{
    function get(){
        return $this->output("hoge");
    }
}
}}
```

な感じで適当に動くやつを作ります。


オリジナルの helper を使う
================================================================================
hepler クラスは自動的にラクダ命名の変数に格納されるっぽいので使うときは autoCss って名前で引っ張ってこれる

そしてコントローラーで

```php
public $helpers = array('autoCss');
```

と明示的に指定してやる・・・

うーん結構ダサいのでこういうものはオリジナルの controller で一気にやってあとは継承してやるのがうまい書き方だな

動くのが確認できたので次


既存のhelperを自作のhelper内から呼び出す
================================================================================
helper クラスの public メンバの helpers に使いたい helper を指定して引っ張ります。

```php
public $helpers = array("Html");
```

controllerではラクダでしたがここではパスカルです。意味不明です。

っで

```php
class AutoCssHelper extends AppHelper{
    public $helpers = array("Html");
    function get(){
        return $this->output($this->Html->css("hoge"));
    }
}
```

こんな感じで使えるようになります。

環境情報の取得
================================================================================
もう流れはできたのであとは、controller名とation名を取得できればOKです。

これは単に html の「文字」を出力するものであって、実際にCSSファイルをローディングするのはブラウザです。なんでエラー処理とか存在とかそういうのどうでもいいんです。調べてみると controller は


```php
$this->params['controller'];
```

actionは

```php
$this->params['action'];
```

っで取得できるようです。

調べてみるとここからいろんな情報が取得できます。一応表にしておく。単にthisをinspectしただけなので・・・

|                      |                 |                                  |
| -------------------- | --------------- | -------------------------------- |
| `base`               | /hoge           | アプリの根っこ後ろスラッシュ無し |
| `webroot`            | /hoge/          | アプリの根っこ後ろスラッシュあり |
| `here`                | /hoge/top/index | リクエストのURL根っこから        |
| `params['controller']` | top             | コントローラー名                 |
| `params['action']`     | index           | action名                         |
| `action`               | index           | action名                         |


完成
================================================================================
ってことで完成

```php
class AutoCssHelper extends AppHelper{
    public $helpers = array("Html");
    function get(){
        return $this->output($this->Html->css($this->params["controller"]. "_" . $this->params["action"]));
    }
}
```

使うと

```html
<link rel="stylesheet" type="text/css" href="/hoge/css/top_index.css" />
```

こんな感じに出力される。OKOK
