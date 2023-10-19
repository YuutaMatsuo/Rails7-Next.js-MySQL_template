# Rails 7 + Next.js + PostegerSQL バックエンド用Dockerfile
FROM ruby:3.2.2
# 公式→https://hub.docker.com/_/ruby

# Rails 7ではWebpackerが標準では組み込まれなくなったので、yarnやnodejsのインストールが不要

# RUN：任意のコマンド実行
RUN mkdir /app

# WORKDIR：作業ディレクトリを指定
WORKDIR /app

# アップデートと必要なパッケージのインストール
RUN apt-get update -qq && apt-get install -y nodejs default-mysql-client

# COPY：コピー元とコピー先を指定
# ローカルのGemfileをコンテナ内の/app/Gemfileに
COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock

#Gemfile.lockにプラットフォームを追記
RUN bundle lock --add-platform ruby
RUN bundle lock --add-platform x86_64-linux

# RubyGemsを最新に更新
RUN gem update && \
    bundle install

# コンテナ起動時に実行させるスクリプトを追加
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# CMD:コンテナ実行時、デフォルトで実行したいコマンド
# Rails サーバ起動
CMD ["rails", "server", "-b", "0.0.0.0"]