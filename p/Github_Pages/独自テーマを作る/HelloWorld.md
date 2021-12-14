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


余談だが、Jekyll のエンジンが markdown の解釈よりも優先して行われるので、 markdown 自体で Jekyll のエンジンの説明をする場合 markdown 全体を `raw`, `endraw` で括らないといけない。


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
  <li><a href="{{ site.baseurl }}">Home</a></li>
</ol>
```
{% endraw %}








