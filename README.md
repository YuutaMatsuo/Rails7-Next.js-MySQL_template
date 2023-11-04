# 1.環境
  frontend
  L Next.js 13
  backend
  L Ruby 3.2.2
  L Rails ~>7.0.5
  db
  L MySQL8

# 2.手順
  ## 2-1 イメージをビルドする
  docker compose build

  ## 2-2 フロントエンド 環境構築
    2-2.1 Next.jsのプロジェクトを作成
    docker compose run --rm frontend yarn create next-app .

    2-2.2 create-next-app後に聞かれている事
      Would you like to use TypeScript with this project?
      このプロジェクトでTypeScriptを使いたいですか？ --yes
      Would you like to use ESLint with this project?
      このプロジェクトでESLintを使いたいですか？  --yes
      Would you like to use Tailwind CSS with this project?
      このプロジェクトでTailwind CSSを使いたいですか？  --yes
      Would you like to use src/ directory with this project?
      このプロジェクトでsrc/ディレクトリを使いたいですか？  --yes
      Use App Router (recommended)?
      App Routerを使いたいですか？（推奨）  --no
      Would you like to customize the default import alias?
      デフォルトのインポートエイリアスをカスタマイズしたいですか？  --no
        What import alias would you like configured?
        設定したいインポートエイリアスは何ですか？

      参考URL:https://zenn.dev/ikkik/articles/51d97ff70bd0da
  
  ## 2-3 バックエンドの環境構築
    2-3.1 Railsのプロジェクトを作成
    docker compose run --rm backend bundle exec rails new . --force --api -d mysql

  ## 2-4 ホストの許可設定
    development.rbに以下の設定を追加します。
    これを追加することでapiホストからのリクエストができるようになります。
    config/environments/development.rb

      Rails.application.configure do
      ・・・
        config.hosts << "backend"
      end

  ## 2-5 dbのユーザー名とパスワードを指定
  config/database.yml に以下を追加
  default: &default
    adapter: mysql2
    encoding: utf8mb4
    pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
    username: <%= ENV["MYSQL_USER"] %>
    password: <%= ENV["MYSQL_PASSWORD"] %>
    host: <%= ENV["MYSQL_HOST"] %>

  development:
    <<: *default
    database: <%= ENV["MYSQL_DATABASE"] %>

  ## 2-6 コンテナの起動
    docker-compose up -d --build

  ## 2-7 データベースの構築
  docker-compose run --rm backend rails db:create

  ## 2-8 正常にビルドされているか確認
    localhost:8000にアクセスしNext.jsの画面が表示されていれば成功
    localhost:3000にアクセスしRailsの画面が表示されていれば成功

