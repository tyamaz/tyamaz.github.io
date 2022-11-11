---
title: Rails7.0 で HelloWorld
aliases:
  - Rails7.0 で HelloWorld
tags:
  - d/2022/04/28
  - n/PGM/Ruby/Rails/v7.0
---

Linux Mint 20.1 環境

準備
================================================================================
[Linux_Mint_に_anyenv_を使って_Ruby_をインストールする](Linux_Mint_に_anyenv_を使って_Ruby_をインストールする.md) で作った環境でやる

- Ruby 3.1
- Rails 7.0.2.3

Rails は `7.0.1` から `Ruby3.1` 対応らしい

Node.js も必要なようなので入れる。[Linux_Mint_に_anyenv_を使って_Node_をインストールする](../29/Linux_Mint_に_anyenv_を使って_Node_をインストールする.md)

SQLite3 も必要そうなので入れる。[Linux_Mint_に_SQLite3_をインストールする](../29/Linux_Mint_に_SQLite3_をインストールする.md)

基本的にガイドに従う [Ruby on Rails Guides](https://guides.rubyonrails.org/)


rails コマンドのインストール
================================================================================

```console
$ gem install rails
```

モタモタやっているうちにバージョンが上がってしまったようだ。

```
$ rails --version
Rails 7.0.2.4
```






Rails アプリケーションの雛形生成
================================================================================

適当なディレクトリにを作ってそこで

```console
$ rails new blog
```

失敗する。何やら SQLite3 関連がダメだと。

```
Could not find gem 'sqlite3 (~> 1.4)' in locally installed gems.
```

だったら入れるってことで

```
$ gem install sqlite3
```

またエラー。調べると `sqlite3.h` が無いからダメっぽいので、`apt search sqlite` で検索してそれっぽいのを探して入れる。


```console
$ apt install libsqlite3-dev
```


再び。

```
$ gem install sqlite3
```

OK

再び。

```console
$ rails new blog
```

OK

`blog` というディレクトリが出来上がっているので移動

```
$ cd blog
```


こんな感じになる。

```
├── Gemfile
├── Gemfile.lock
├── README.md
├── Rakefile
├── app
│   ├── assets
│   │   ├── config
│   │   │   └── manifest.js
│   │   ├── images
│   │   └── stylesheets
│   │       └── application.css
│   ├── channels
│   │   └── application_cable
│   │       ├── channel.rb
│   │       └── connection.rb
│   ├── controllers
│   │   ├── application_controller.rb
│   │   └── concerns
│   ├── helpers
│   │   └── application_helper.rb
│   ├── javascript
│   │   ├── application.js
│   │   └── controllers
│   │       ├── application.js
│   │       ├── hello_controller.js
│   │       └── index.js
│   ├── jobs
│   │   └── application_job.rb
│   ├── mailers
│   │   └── application_mailer.rb
│   ├── models
│   │   ├── application_record.rb
│   │   └── concerns
│   └── views
│       └── layouts
│           ├── application.html.erb
│           ├── mailer.html.erb
│           └── mailer.text.erb
├── bin
│   ├── bundle
│   ├── importmap
│   ├── rails
│   ├── rake
│   └── setup
├── config
│   ├── application.rb
│   ├── boot.rb
│   ├── cable.yml
│   ├── credentials.yml.enc
│   ├── database.yml
│   ├── environment.rb
│   ├── environments
│   │   ├── development.rb
│   │   ├── production.rb
│   │   └── test.rb
│   ├── importmap.rb
│   ├── initializers
│   │   ├── assets.rb
│   │   ├── content_security_policy.rb
│   │   ├── filter_parameter_logging.rb
│   │   ├── inflections.rb
│   │   └── permissions_policy.rb
│   ├── locales
│   │   └── en.yml
│   ├── master.key
│   ├── puma.rb
│   ├── routes.rb
│   └── storage.yml
├── config.ru
├── db
│   └── seeds.rb
├── lib
│   ├── assets
│   └── tasks
├── log
│   └── development.log
├── public
│   ├── 404.html
│   ├── 422.html
│   ├── 500.html
│   ├── apple-touch-icon-precomposed.png
│   ├── apple-touch-icon.png
│   ├── favicon.ico
│   └── robots.txt
├── storage
├── test
│   ├── application_system_test_case.rb
│   ├── channels
│   │   └── application_cable
│   │       └── connection_test.rb
│   ├── controllers
│   ├── fixtures
│   │   └── files
│   ├── helpers
│   ├── integration
│   ├── mailers
│   ├── models
│   ├── system
│   └── test_helper.rb
├── tmp
│   ├── cache
│   │   ├── assets
│   │   └── bootsnap
│   │       ├── compile-cache-iseq ※キャッシュファイル大量
│   │       │   ├── 00
│   │       │   │   ├── 0d313cd8ffa4ac
│   │       │   └── ff
│   │       │       ├── 1f6538a53ad0fc
│   │       │       ├── 3b9cabd150ee0a
│   │       │       ├── 98b0d089593948
│   │       │       ├── 99bdb2c21a7fbe
│   │       │       ├── ba2adddcb2c7ca
│   │       │       ├── cdd60c9041f53b
│   │       │       ├── d2017dc421d41a
│   │       │       ├── da20fcc60842a5
│   │       │       └── fc85087cf4cc64
│   │       └── load-path-cache
│   ├── development_secret.txt
│   ├── pids
│   └── storage
└── vendor
    └── javascript
```


開発用サーバの起動

```console
$ bin/rails server
```

起動した後

```
http://127.0.0.1:3000
```

へブラウザでアクセスすると、こうなれば OK


![](Pasted%20image%2020220429162925.png)


URL のルーティング
================================================================================

`config/routes.rb` を開く。↓のようになっている。


```ruby
Rails.application.routes.draw do
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"
end
```

ここに 追記する。

```ruby
 get "/articles", to: "articles#index"
```

こうなる


```ruby
Rails.application.routes.draw do
  get "/articles", to: "articles#index"
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"
end
```

`articles#index` へルーティングしたので対象のコントローラーを作る

これは自動生成でやる

```console
$ bin/rails generate controller Articles index --skip-routes
```

↓らしい

```console
      create  app/controllers/articles_controller.rb
      invoke  erb
      create    app/views/articles
      create    app/views/articles/index.html.erb
      invoke  test_unit
      create    test/controllers/articles_controller_test.rb
      invoke  helper
      create    app/helpers/articles_helper.rb
      invoke    test_unit
```


見てみる。

`app/controllers/articles_controller.rb`

```ruby
class ArticlesController < ApplicationController
  def index
  end
end
```

↑名前だけで実装無し。

`app/views/articles/index.html.erb`

```html
<h1>Articles#index</h1>
<p>Find me in app/views/articles/index.html.erb</p>
```

↑生成された HTML の断片



`test/controllers/articles_controller_test.rb`

```ruby
require "test_helper"

class ArticlesControllerTest < ActionDispatch::IntegrationTest
  # test "the truth" do
  #   assert true
  # end
end
```

↑テストらしいが中身なし



`app/helpers/articles_helper.rb`

```ruby
module ArticlesHelper
end
```

何か


とりあえずガワだけ出来上がった感はある。

サーバを起動し

```console
$ bin/rails server
```

設定した URLへアクセスしてみる

```
http://localhost:3000/articles
```

このように表示される。

```
Articles#index
Find me in app/views/articles/index.html.erb
```


まとめると

- `http://localhost:3000/articles` でアクセス
- `config/routes.rb` の
- `get "/articles", to: "articles#index"` で `/article` で来たら `articles#index` という









