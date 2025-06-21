🚀 実装ロードマップ（Karunekkoma）

🎯 目標

- 必要最低限の「登録 → 一覧 → 編集」が成り立つMVPを完成させる
- Nuxt 3（フロント） × Rails API（バック） × Tailwind CSS（デザイン）

---

🗂 フェーズ分けで考えるとこんな感じ👇

🧱 Phase 0：セットアップ（準備）

- [ ] GitHubリポジトリ2つ作成（frontend / backend）
- [ ] Dockerで開発環境構築（バックエンド側）
- [ ] Nuxt 3 プロジェクト作成（TypeScript / Pinia / Tailwind導入）

---

💻 Phase 1：バックエンド（Rails API）

- [ ] Rails 7 (--api) new
- [ ] PostgreSQL セットアップ
- [ ] `children`リソース用のモデル・マイグレーション作成（name, birth_date, memo）
- [ ] Controller作成（CRUD: index, create, update, destroy, show）
- [ ] RailsのCORS設定（Nuxtと連携用）
- [ ] APIテストでcurl / httpie / Postmanで確認

---

🎨 Phase 2：フロントエンド構築（Nuxt 3）

- [ ] Nuxt 3 + Tailwind CSS 初期化
- [ ] ルーティング（pages/children/index.vue など）
- [ ] 共通レイアウト（layouts/default.vue）作成
- [ ] Navigation Bar 実装（一覧 / 新規追加リンク）

---

📄 Phase 3：CRUD機能作成

- [ ] 一覧表示ページ（GET /children）
- [ ] 新規登録ページ（POST /children）
  - vee-validate & yup バリデーション適用
- [ ] 編集ページ（PATCH /children/:id）
  - フォームの初期値設定
- [ ] 削除処理のUI追加（DELETE対応）
- [ ] API接続 → AxiosでRails APIと通信

---

🎀 Phase 4：デザイン調整 & 温かみあるUI化

- [ ] テーマカラー適用（サンドイエロー系 Tailwindカスタム）
- [ ] カード型の一覧デザイン
- [ ] ボタン・入力フォームデザイン調整
- [ ] エラーメッセージや通知系のUI導入（簡易トーストでもOK）

---

🚀 Phase 5：Deploy（公開）

- [ ] フロント：Vercelへデプロイ（Nuxt）
- [ ] バックエンド：RenderでRails + DBデプロイ
- [ ] Nuxtの.envにAPI_BASE_URL設定
- [ ] 動作確認（フォーム→登録→一覧確認まで）

---

🔮 Phase 6：必要あれば追加

- [ ] 詳細画面（/children/:id）
- [ ] 404エラーページ
- [ ] カレンダー画面（誕生日ビジュアル化など）

---

✅ 完了の定義（MVP DONEライン）

- [ ] 子どもを登録できる（バリデーション付き）
- [ ] 登録された子どもが一覧で見れる（名前・誕生日・年齢・メモ）
- [ ] 既存データの編集・削除ができる
- [ ] UIが温かみある色合いで整っている
- [ ] どのデバイスでも見れる（レスポンシブ）
- [ ] 本番ホスティングで動いている（Vercel + Render）
