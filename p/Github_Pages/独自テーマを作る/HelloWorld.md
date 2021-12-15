---
render_with_liquid: false
title: Github Pages / 独自テーマを作る / HelloWorld
---

2021-12-14 付近での話

独自のテンプレートを作る
================================================================================

ルートディレクトリに `_layouts` というディレクトリを設置する。

その中に `default.html` というファイルを作りそこにこのように書き込む


{% raw %}


```html
<!DOCTYPE html>
<html>
  <body>
    {{ content }}
  </body>
</html>
```

この `{{ content }}` 部分に markdown ファイルから変換された HTML が埋まることになる。

{% endraw %}


余談だが、Jekyll の `Liquid` 解釈が markdown の解釈よりも優先して行われるので、 markdown 自体で Jekyll の説明を書く場合 markdown 全体を `raw`, `endraw` で括らないといけない。

markdown のファイル中の `Liquid` コードを一括で無効化するオプションは無いっぽい。

`Jekyll 4.0` 以降は、該当のドキュメントの `frontmatter` にこのように記述すると抑止できるっぽい。

```yml
render_with_liquid: false
```

しかし、2021-12-14 現在、Github Pages の Jekyll は `3.9` っぽいのでこれは機能しない。[Jekyll 4 Released](https://jekyllrb.com/news/2019/08/20/jekyll-4-0-0-released/), [Dependency versions GitHub Pages](https://pages.github.com/versions/)

このリリースが2年も前なので、互換性の問題でどうにもならん問題があるんだろうと想像できる。





シンタックスハイライトを設定する
================================================================================
このままだとシンタックスハイライトが付かないのでそれは不味いのでつける。
`gruvbox` が好きなのでそれをやってみる


`Github Pages` は `rouge` という Ruby ライブラリでハイライトされているようで、その付属ツールである `rougify` でそのハイライト用のコードを吐き出せるようだ。`Ruby` や `rouge` のインストールに関しては他で。

とりあえず内蔵テーマを見る。

```console
$ rougify help style
usage: rougify style [<theme-name>] [<options>]

Print CSS styles for the given theme.  Extra options are
passed to the theme. To select a mode (light/dark) for the
theme, append '.light' or '.dark' to the <theme-name>
respectively. Theme defaults to thankful_eyes.

options:
  --scope     	(default: .highlight) a css selector to scope by
  --tex       	(default: false) render as TeX
  --tex-prefix	(default: RG) a command prefix for TeX
              	implies --tex if specified

available themes:
  base16, base16.dark, base16.light, base16.monokai,
  base16.monokai.dark, base16.monokai.light, base16.solarized,
  base16.solarized.dark, base16.solarized.light, bw, colorful,
  github, gruvbox, gruvbox.dark, gruvbox.light, igorpro, magritte,
  molokai, monokai, monokai.sublime, pastie, thankful_eyes, tulip
```

ということで `gruvbox` があるので、こいつを吐き出す

```
$ rougify style gruvbox > gruvbox.css
```

こんなの CDN ありそうなんだが無いっぽい。

テーマでは `/assets/css/style.css` のスタイルをまず読み込むのが作法っぽい。

そのために、 `/asset/css/style.scss` というファイルを作る。`Github Pages` の機能で `.scss` が `css` にコンパイルされるっぽい。[Sass: Sass Basics](https://sass-lang.com/guide)

`style.scss` ファイルにさっきの `gruvbox.css` の内容を書き込む、`SCSS` は `CSS` と文法に互換があるので、`CSS` は上手くない `SCSS` として通用する。以下のようになる。

```css
---
---

.highlight, .highlight .w {
  color: #fbf1c7;
  background-color: #282828;
}
.highlight .err {
  color: #fb4934;
  background-color: #282828;
  font-weight: bold;
}
.highlight .c, .highlight .ch, .highlight .cd, .highlight .cm, .highlight .cpf, .highlight .c1, .highlight .cs {
  color: #928374;
}
.highlight .cp {
  color: #8ec07c;
}
.highlight .nt {
  color: #fb4934;
}
.highlight .o, .highlight .ow {
  color: #fbf1c7;
}
.highlight .p, .highlight .pi {
  color: #fbf1c7;
}
.highlight .gi {
  color: #b8bb26;
  background-color: #282828;
}
.highlight .gd {
  color: #fb4934;
  background-color: #282828;
}
.highlight .gh {
  color: #b8bb26;
  font-weight: bold;
}
.highlight .k, .highlight .kn, .highlight .kp, .highlight .kr, .highlight .kv {
  color: #fb4934;
}
.highlight .kc {
  color: #d3869b;
}
.highlight .kt {
  color: #fabd2f;
}
.highlight .kd {
  color: #fe8019;
}
.highlight .s, .highlight .sb, .highlight .sc, .highlight .dl, .highlight .sd, .highlight .s2, .highlight .sh, .highlight .sx, .highlight .s1 {
  color: #b8bb26;
}
.highlight .si {
  color: #b8bb26;
}
.highlight .sr {
  color: #b8bb26;
}
.highlight .sa {
  color: #fb4934;
}
.highlight .se {
  color: #fe8019;
}
.highlight .nn {
  color: #8ec07c;
}
.highlight .nc {
  color: #8ec07c;
}
.highlight .no {
  color: #d3869b;
}
.highlight .na {
  color: #b8bb26;
}
.highlight .m, .highlight .mb, .highlight .mf, .highlight .mh, .highlight .mi, .highlight .il, .highlight .mo, .highlight .mx {
  color: #d3869b;
}
.highlight .ss {
  color: #83a598;
}
```

斜体のスタイルが邪魔なので除去した。
この エントリポイントとなる `SCSS` を書く場合に忘れてはいけないのがファイルの冒頭に `Frontmatter` が必要になるということ。冒頭のハイフンの区切り2行はそういう意味になる。これが無いと `Github Pages` にコンパイルされずに `CSS` として認識されない。

`default.html` にこいつを書き加える


```html
<link rel="stylesheet" href="/assets/css/style.css" />
```

ここで、もし何らかの原因で `style.css` が読み込めなかった場合にも、適当な `style.css` が自動生成されて、勝手にデフォルトのスタイルが当たってしまうので、自分の書いたスタイルじゃないと思ったら、それを疑うとよい。


パンくずリストを作る
================================================================================
パンくずリストを作る。`_layouts/default.html` へまずこのように追加する。

{% raw %}
```html
<ol class="breadcrumbs">
  <li><a href="{{ site.github.url }}">Home</a></li>
</ol>
```
{% endraw %}


リンクがちゃんと表示された。

次に後続のリンクを並べてみる。今回のサイトは、ディレクトリ構造と URL が完全に一致している、書くディレクトリに必ず `index.md` がある前提として作る。

パンくずの階層情報は URL からそのまま取る。`page.url` という変数には最終的に確定した対象のページの URL そのものが入ってくる(`/hoge/piyo/fuga.html` みたいな形)ので、こいつをスラッシュで分解して順番に結合すればパンくずリストになる。多分。

このようにすることで記述することができた。

{% raw %}
```html
    <ol class="breadcrumbs">
      <li><a href="{{ site.github.url }}">Home</a></li>
      {% assign crumbs = page.url | split: '/' %}
      {% assign crumburl = '' %}
      {% for crumb in crumbs offset: 1 %}
        {% assign crumburl = crumburl | append: '/' | append: crumb %}
        {% if forloop.last %}
        {% else %}
          <li><a href="{{ crumburl }}">{{ crumb | url_decode }}</a></li>
        {% endif %}
      {% endfor %}
    </ol>
```
{% endraw %}

まず、パンくずというのは順番に並んだリンクのリストという考えなので `ol` タグで実装する。明示的に `class` にもパンくずであることを記述しておく。

`Jetkyll` では `Liquid` と呼ばれるテンプレートエンジン記述を使う。ここでブラケットパーセントで挟まっているところが `Liquid` の記述となる。[Liquid template language](https://shopify.github.io/liquid/)

まず `page.url` で得られるスラッシュ区切りの文字列を `split` の `filter` でスラッシュ指定でバラバラに分解し配列にして `assign` というキーワードを使って `crumbs` という変数に詰めている。

- [Variable – Liquid template language](https://shopify.github.io/liquid/tags/variable/)
- [split – Liquid template language](https://shopify.github.io/liquid/filters/split/)

`Liquid` では値に対して何か処理を加えるモノを `filter` と言って、シェルスクリプトのように `|` で繋ぐことで組み合わせる。

次にみんな大好きな foreach をやるために `for ... in ...` という構文を使っている。[Iteration – Liquid template language](https://shopify.github.io/liquid/tags/iteration/)

`crumbs` の内容が1個1個 `crumb` に代入されながらループするというお馴染みの構文である。`offset` という記述があるが、これはスタートのインデックスを1個だけ後ろにズラして開始するということになる。何故かというと、`page.url` で得られる内容はスラッシュで始まるので最初の1個目が必ず空になってしまうからである。

次に、`append` filter で文字列を結合している。`crumburl` という変数に対してスラッシュで積み上げていくことでパンくずの URL を生成することをしている。

最後にそれらを使って `a` タグを組み立てている。日本語名が含まれる URL は [urlエンコード](https://ja.wikipedia.org/wiki/%E3%83%91%E3%83%BC%E3%82%BB%E3%83%B3%E3%83%88%E3%82%A8%E3%83%B3%E3%82%B3%E3%83%BC%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0)されているので、表示の部分だけ `url_decode` filter を使って元に戻している。[url\_decode – Liquid template language](https://shopify.github.io/liquid/filters/url_decode/)

`forloop` というのは `for` のブロック内部で使える暗黙的な変数でここからループ状態を取ることができる。今回は `forloop.last` を使って最後のループか否かを判定している。`page.url` の最後の部分を指すので、つまりそれはこのページ自身なのでパンくずとして不要なので飛ばすということである。


CSS を整理する
================================================================================
パンくずのスタイルを書くにあたって現状 `style.scss` に全部ベタ書きしているモノをやや整理する。

`_sass` というディレクトリを作ってここに `SCSS` のファイルを設置して、そいつを `import` するというのが定番のやり方のようだ。間違えないように `_scss` ではなく `_sass` である。`SASS` というのは、`CSS` 生成用の簡易記述言語みたいなもので、記述性が重視されていて `CSS` と互換性が無い。それでは都合が悪いことが多くて後に `SASS` の拡張仕様として登場したのが `SCSS` ということになる。なので環境としては `SCSS` というのは `SASS` の一部ということになる。間違えやすい。

この前のコードハイライトの CSS を `_sass/_code_highlight.scss` として、切り出す。ファイル名の先頭にアンダースコアが付いているのは、こうしておくことで、単体の SCSS ファイルとは見なされなくなり、単独コンパイルされて  CSS ファイルが生成されなくなる。つまり import して使われる専用のファイルとなる。


ついでに [css2sass \| Convert CSS Snippets to Syntactically Awesome StyleSheets code](http://css2sass.herokuapp.com/) こういうツールがあったので、現状の CSS 記述を SCSS 記述に変換してみる。

```scss
.highlight {
  color: #fbf1c7;
  background-color: #282828;

  .w {
    color: #fbf1c7;
    background-color: #282828;
  }

  .err {
    color: #fb4934;
    background-color: #282828;
    font-weight: bold;
  }

  .c, .ch, .cd, .cm, .cpf, .c1, .cs {
    color: #928374;
  }

  .cp {
    color: #8ec07c;
  }

  .nt {
    color: #fb4934;
  }

  .o, .ow, .p, .pi {
    color: #fbf1c7;
  }

  .gi {
    color: #b8bb26;
    background-color: #282828;
  }

  .gd {
    color: #fb4934;
    background-color: #282828;
  }

  .gh {
    color: #b8bb26;
    font-weight: bold;
  }

  .k, .kn, .kp, .kr, .kv {
    color: #fb4934;
  }

  .kc {
    color: #d3869b;
  }

  .kt {
    color: #fabd2f;
  }

  .kd {
    color: #fe8019;
  }

  .s, .sb, .sc, .dl, .sd, .s2, .sh, .sx, .s1, .si, .sr {
    color: #b8bb26;
  }

  .sa {
    color: #fb4934;
  }

  .se {
    color: #fe8019;
  }

  .nn, .nc {
    color: #8ec07c;
  }

  .no {
    color: #d3869b;
  }

  .na {
    color: #b8bb26;
  }

  .m, .mb, .mf, .mh, .mi, .il, .mo, .mx {
    color: #d3869b;
  }

  .ss {
    color: #83a598;
  }
}
```

ぐっと読みやすく整理された。

そして `style.scss` をこのように書き換える。

```scss
---
---

@import '_code_highlight';
```

これでスタイルの分割管理の方法がわかった。

なので今度はパンくずのスタイルも書き込んでおこう

`_sass/_breadcrumbs.scss` を作ろう。

```scss
ol.breadcrumbs{
    display: flex;
    list-style-type: none;
    li{
        margin-right: 1rem;
    }
}
```


当然、`style.scss` はこうなる。

```scss
---
---

@import
  '_code_highlight',
  '_breadcrumbs';
```

title をつける
================================================================================
ページごとにタイトルをつけられるようにする。

URLとタイトルが完全対応するわけで無い場合も多いので独自に設定できるようにする。


これは簡単で、対象の markdown ファイルの `frontmatter` に対して

```yml
title: hogehoge
```

と書けばいい。

これを `h1` タグと `title` タグで埋め込むように `default.html` を編集する。

こんな感じで。
{% raw %}
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
    <title>{{ page.title }}</title>
```
{% nedraw %}
ページ制御のための定番のメタ情報をいくつか追加しておく。





























