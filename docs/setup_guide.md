# Rails 8 API + Vue 3 + Tailwind CSS + Docker セットアップガイド

このドキュメントでは、Rails 8のAPIモード、Vue 3、Tailwind CSS、Dockerを使用したアプリケーション環境の構築から実行までの詳細な手順を説明します。

## 目次

1. [前提条件](#前提条件)
2. [プロジェクト構造](#プロジェクト構造)
3. [セットアップ手順](#セットアップ手順)
4. [開発フロー](#開発フロー)
5. [トラブルシューティング](#トラブルシューティング)

## 前提条件

以下のソフトウェアがインストールされていることを確認してください：

- Docker
- Docker Compose

その他の依存関係（Ruby、Node.js、PostgreSQLなど）はDockerコンテナ内で管理されるため、ローカルにインストールする必要はありません。

## プロジェクト構造

```
rails8_vue3_project/
├── backend/                # Rails APIアプリケーション
│   ├── Dockerfile
│   ├── Gemfile
│   ├── Gemfile.lock
│   ├── app/
│   │   └── controllers/
│   │       └── api/
│   │           └── v1/
│   │               └── greetings_controller.rb
│   └── config/
│       ├── database.yml
│       ├── initializers/
│       │   └── cors.rb
│       └── routes.rb
├── frontend/               # Vue 3アプリケーション
│   ├── Dockerfile
│   ├── index.html
│   ├── package.json
│   ├── postcss.config.js
│   ├── tailwind.config.js
│   └── src/
│       ├── App.vue
│       ├── main.js
│       ├── assets/
│       │   └── main.css
│       ├── components/
│       ├── router/
│       │   └── index.js
│       ├── services/
│       │   └── api.js
│       ├── stores/
│       └── views/
│           └── HomeView.vue
├── docker-compose.yml      # 開発環境の設定
├── .env                    # 環境変数
└── test_application.sh     # テストスクリプト
```

## セットアップ手順

### 1. リポジトリのクローン

```bash
git clone <repository-url> rails8_vue3_project
cd rails8_vue3_project
```

または、新しいプロジェクトを作成する場合：

```bash
mkdir -p rails8_vue3_project
cd rails8_vue3_project
# このドキュメントの手順に従ってファイルを作成
```

### 2. 環境変数の設定

`.env`ファイルを確認し、必要に応じて環境変数を調整します：

```
POSTGRES_PASSWORD=password
POSTGRES_USER=postgres
POSTGRES_DB=app_development
DATABASE_URL=postgres://postgres:password@db:5432/app_development
RAILS_ENV=development
VITE_API_URL=http://localhost:3000
```

### 3. Dockerコンテナのビルドと起動

```bash
docker-compose build
docker-compose up
```

初回起動時には、以下のコマンドでデータベースを作成する必要があります：

```bash
docker-compose run backend rails db:create db:migrate
```

### 4. アプリケーションへのアクセス

- バックエンドAPI: <http://localhost:3000/api/v1/greetings>
- フロントエンド: <http://localhost:5173>

## 開発フロー

### バックエンド開発（Rails API）

#### モデルの作成

```bash
docker-compose run backend rails generate model User name:string email:string
docker-compose run backend rails db:migrate
```

#### コントローラーの作成

```bash
docker-compose run backend rails generate controller Api::V1::Users index show create update destroy
```

#### ルーティングの設定

`backend/config/routes.rb`を編集：

```ruby
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :users
      get 'greetings', to: 'greetings#index'
    end
  end
end
```

### フロントエンド開発（Vue 3）

#### 新しいコンポーネントの作成

`frontend/src/components/`ディレクトリに新しい`.vue`ファイルを作成します。

#### 新しいビューの作成

1. `frontend/src/views/`ディレクトリに新しい`.vue`ファイルを作成
2. `frontend/src/router/index.js`にルートを追加：

```javascript
import NewView from '../views/NewView.vue'

const routes = [
  // 既存のルート
  {
    path: '/new-path',
    name: 'new-view',
    component: NewView
  }
]
```

#### APIサービスの拡張

`frontend/src/services/api.js`を編集して新しいAPIエンドポイントを追加：

```javascript
export default {
  // 既存のメソッド
  getUsers() {
    return apiClient.get('/api/v1/users')
  },
  getUser(id) {
    return apiClient.get(`/api/v1/users/${id}`)
  }
}
```

## トラブルシューティング

### よくある問題と解決策

#### CORS関連のエラー

フロントエンドからAPIへのリクエストがCORSエラーで失敗する場合：

1. `backend/config/initializers/cors.rb`の設定を確認
2. 許可されているオリジンが正しいことを確認（`origins 'localhost:5173'`）

#### データベース接続エラー

データベース接続に問題がある場合：

1. `.env`ファイルの設定を確認
2. `backend/config/database.yml`の設定を確認
3. データベースコンテナが起動していることを確認：`docker-compose ps`

#### フロントエンドからAPIへの接続エラー

フロントエンドがAPIに接続できない場合：

1. 環境変数`VITE_API_URL`が正しく設定されていることを確認
2. APIサーバーが起動していることを確認
3. ブラウザのコンソールでネットワークエラーを確認

### デバッグ方法

#### バックエンドのログ確認

```bash
docker-compose logs backend
```

#### フロントエンドのログ確認

```bash
docker-compose logs frontend
```

#### データベースのログ確認

```bash
docker-compose logs db
```

#### コンテナ内でのコマンド実行

```bash
docker-compose exec backend bash
docker-compose exec frontend sh
```

## 本番環境へのデプロイ

本番環境へのデプロイについては、以下の点に注意してください：

1. 本番用の環境変数を適切に設定
2. フロントエンドのビルド：`docker-compose run frontend npm run build`
3. バックエンドの設定：`RAILS_ENV=production`
4. セキュリティ対策（APIキー、シークレットの管理など）

詳細なデプロイ手順については、プロジェクトの要件に応じて別途ドキュメントを作成することをお勧めします。
