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


余談としてだが、Jekyll のエンジンが markdown の解釈よりも優先して行われるので、 markdown 自体で Jekyll のエンジンの説明をする場合 markdown 全体を `raw` `endraw` で括らないといけない。


