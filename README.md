# rails6_mysql_docker
docker + rails6 + mysql

## rails new でアプリ作成
5つのファイルが用意できたら、ターミナルから下記コマンドを実行。
```
docker-compose run web rails new . --force --no-deps --database=mysql --skip-test --webpacker
```
- あとで RSpec を導入するために--skip-test オプションで Minitest をスキップ。<br>
- --webpacker オプションで webpacker をインストール。

## イメージのビルド
rails new で各種ファイルの作成、webpacker のインストールが完了したら、<br>
Gemfile が更新されているのため、イメージをビルド。

```
docker-compose build
```

## database.yml の設定と DB 接続
rails new で作成された config/database.yml を編集

```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV.fetch("MYSQL_USERNAME", "root") %>
  password: <%= ENV.fetch("MYSQL_PASSWORD", "password") %>
  host: <%= ENV.fetch("MYSQL_HOST", "db") %>

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
  username: myapp
  password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>
```

データベースの作成<br>
```
docker-compose run web rake db:create
```

## コンテナ起動
```
docker-compose up
```
localhost:3000 にアクセスすると例のページが表示
