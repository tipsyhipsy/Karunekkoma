🧱 Karunekkoma — 技術スタック一覧

🖥 フロントエンド（UI・ロジック）

- Nuxt 3（Vue 3 ベースのモダンFW）
- TypeScript（型安全💪）
- Pinia（状態管理）
- Tailwind CSS（スタイリング）
- vee-validate + yup（フォームバリデーション）
- Axios（API通信）
- Heroicons（UI用アイコン：任意）
- Vite（ビルドツール）

🧠 バックエンド（API）

- Ruby on Rails 7（API mode）
- PostgreSQL（DB）
- ActiveModelSerializer（JSON整形用：必要に応じて）
- rack-cors（CORS設定調整）

🐳 開発環境

- Docker（開発環境の統一）
- docker-compose（Rails + DB 起動制御）
- Git / GitHub（バージョン管理・CI/CD連携用）
- pre-commit（必要ならコード整形自動化）

🛠 開発支援ツール（任意）

- ESLint + Prettier（コード整形 & 静的解析）
- Volar（Nuxt + TS用のIDE補助：VSCode拡張）

🌐 デプロイ・ホスティング

・フロントエンド（Nuxt）：

- Vercel（無料枠あり・爆速デプロイ）

・バックエンド（Rails API）：

- Render（GUIで管理しやすい・DB付き）

・データベース管理：

- PostgreSQL（Render内で自動管理）

🔑 環境変数管理

- Render の秘密管理機能（Rails用：RAILS_MASTER_KEY、DATABASE_URLなど）
- Nuxt `.env`（API_BASE_URL設定など）

🎨 デザイン方針

- テーマ：サンドイエロー / ゴールド系
- メインカラー：`#EEC373`
- 背景カラー：`#F6F1E7`
- アクセントブラウン：`#7C5B3A`
