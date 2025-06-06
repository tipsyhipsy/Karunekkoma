🏗 Renderのための Rails 7 (APIモード) 初期デプロイ構成案

🎯 目的

- Rails APIバックエンドをRenderホスティングにデプロイ
- PostgreSQL DBもRenderで管理
- NuxtからのAPIリクエストがRender経由でちゃんと通るようにする（CORS対応）

🎁 想定条件

- Rails：7.x（--api モード）
- DB：PostgreSQL
- ビルドツール：Docker なしでも可（Heroku互換方式）
- フロント（Nuxt）は別ホスティング（Vercelなど）

✅ 事前準備

- GitHub連携済のRailsリポジトリ
- `Gemfile` に pg 入ってる？
- `config/database.yml` はデフォルトで OK（自動でRenderが環境変数設定）
- `config/environments/production.rb` で `host` 設定しとこ！

📝 手順（基本パターン）

1. Renderダッシュボード ▶️ "New Web Service"を選択
2. GitHubのRailsリポジトリを選択
3. 各設定を以下のように👇

🛠 Service設定

| 項目               | 設定内容（例）                  |
|--------------------|-------------------------------|
| Name              | babylog-api（自由）            |
| Runtime           | Ruby                           |
| Build Command     | `bundle install && bundle exec rake db:migrate`（or `bin/setup`）|
| Start Command     | `bundle exec puma -C config/puma.rb`（だいたいこれ） |
| Branch            | `main`                         |
| Region            | Singapore（or Tokyoがあれば） |

🛠 Database追加

- RenderのAdd-onから PostgreSQL を追加（Railsと同じプロジェクト内で）
- 自動で `DATABASE_URL` 環境変数が作成される＆Railsが認識

🛠 環境変数設定（Environment → Add）

| KEY              | VALUE                                                                 |
|------------------|------------------------------------------------------------------------|
| RAILS_ENV        | production                                                             |
| DATABASE_URL     | 自動発行されるのでOK                                                  |
| SECRET_KEY_BASE  | `rails secret` で生成したやつ貼る                                     |
| RAILS_MASTER_KEY | マスタキー（credentials対応してる場合）                              |
| CORS_ORIGINS     | `https://{あなたのNuxtホスト}.vercel.app` （フロントと通信するなら） |

✨ CORSの設定例（config/initializers/cors.rb）

```rb
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins ENV.fetch("CORS_ORIGINS", "*")
    resource "*", headers: :any, methods: [:get, :post, :patch, :put, :delete, :options]
  end
end
