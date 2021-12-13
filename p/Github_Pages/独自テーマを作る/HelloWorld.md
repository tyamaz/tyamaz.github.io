ルートディレクトリに `_layouts` というディレクトリを設置する。

その中に `default.html` というファイルを作りそこにこのように書き込む


```html
<!DOCTYPE html>
<html>
  <body>
    {{ content }}
  </body>
</html>
```

この `{{ content }}` 部分に markdown ファイルから変換された HTML が埋まることになる。
