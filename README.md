# WooCommerce ドキュメント日本語翻訳システム

WooCommerce公式ドキュメントを自動的に日本語に翻訳し、独自サイトで公開するための完全自動化システムです。

## 🚀 特徴

- **自動同期**: WooCommerceドキュメントの更新を毎日チェック
- **PRベースレビュー**: 翻訳結果を人間がレビューできるワークフロー
- **API最適化**: DeepL無料枠（月50万文字）を効率的に使用
- **バージョン管理**: WooCommerceのバージョンごとに翻訳を管理
- **優先度付き翻訳**: 重要なドキュメントから優先的に翻訳
- **翻訳キャッシュ**: 同じ内容の再翻訳を防止

## 📋 必要な環境

- Node.js 18以上
- GitHub リポジトリ
- DeepL API キー（無料版でOK）
- ホスティングサービス（Vercel、Netlify等）

## 🛠️ セットアップ

### 1. リポジトリのクローン

```bash
git clone https://github.com/your-org/woocommerce-docs-ja.git
cd woocommerce-docs-ja
npm install
```

### 2. 環境変数の設定

GitHub Secretsに以下を設定:

- `DEEPL_API_KEY`: DeepL APIキー
- `VERCEL_TOKEN`: Vercelデプロイ用トークン（オプション）

### 3. 初回セットアップ

```bash
# API使用量の確認
npm run check:api

# 初回の変更検出
npm run detect:changes

# 手動で翻訳を実行（テスト）
DEEPL_API_KEY=your-key npm run translate -- --usage 0 --limit 500000 --version 8.5.0
```

## 📊 使い方

### 自動実行

毎週月曜日の午前2時（JST）に自動的に実行されます。

### 手動実行

GitHub Actionsから手動実行も可能:

1. Actions タブを開く
2. "Translation Review Process" を選択
3. "Run workflow" をクリック

### コマンド一覧

```bash
# API使用量の確認
npm run check:api

# 変更ファイルの検出
npm run detect:changes

# 翻訳レポートの生成
npm run report

# バージョン管理
npm run version:current     # 現在のバージョンを表示
npm run version:history     # バージョン履歴を表示
npm run version:switch 8.4.0  # 特定バージョンに切り替え
npm run version:diff 8.4.0 8.5.0  # バージョン間の差分を表示

# 開発
npm run dev       # 開発サーバー起動
npm run build     # ビルド
npm run preview   # プレビュー
```

## 🔄 ワークフロー

### 1. 変更検出フェーズ
- WooCommerceリポジトリから最新ドキュメントを取得
- 前回の翻訳から変更されたファイルを検出
- DeepL API使用量をチェック

### 2. 翻訳フェーズ
- 優先度順にファイルをソート
- API制限内で可能な限り翻訳
- 翻訳結果をキャッシュに保存

### 3. レビューフェーズ
- 翻訳結果でPRを作成
- プレビュー環境にデプロイ
- 人間によるレビュー

### 4. デプロイフェーズ
- PRがマージされたら本番環境にデプロイ
- バージョン情報を記録

## 📈 API使用量の管理

### 月間制限
- 無料プラン: 500,000文字/月
- 使用量が80%を超えると警告

### 最適化戦略
1. **翻訳キャッシュ**: 同じテキストの再翻訳を防止
2. **優先度付け**: 重要なドキュメントを優先
3. **セグメント分割**: 大きなファイルを効率的に処理
4. **バッチ処理**: API制限内でバッチを作成

### 使用量の確認

```bash
# 簡易表示
npm run check:api

# 詳細レポート
node scripts/check-api-usage.js detailed

# JSON形式
node scripts/check-api-usage.js json
```

## 🔍 トラブルシューティング

### API制限に達した場合

1. 翌月まで待つ
2. 優先度の高いファイルのみを手動で選択
3. 有料プランへのアップグレードを検討

### 翻訳品質の問題

1. `technicalGlossary`に専門用語を追加
2. 翻訳後の手動修正をPRで実施
3. キャッシュをクリアして再翻訳

### バージョン不整合

```bash
# 現在のバージョンを確認
npm run version:current

# 特定バージョンに切り替え
npm run version:switch 8.4.0

# 差分を確認
npm run version:diff 8.4.0 8.5.0
```

## 📝 カスタマイズ

### 優先度の変更

`scripts/optimized-translate.js`の`priorityMap`を編集:

```javascript
const priorityMap = {
  'getting-started': 10,  // 最高優先度
  'installation': 9,
  // ... カスタマイズ
};
```

### 専門用語の追加

`scripts/optimized-translate.js`の`technicalGlossary`を編集:

```javascript
const technicalGlossary = {
  'WooCommerce': 'WooCommerce',
  'your-term': 'あなたの用語',
  // ... 追加
};
```

### 翻訳エンジンの変更

DeepL以外のAPIを使用する場合は、`translateText`関数を修正してください。

## 🤝 コントリビューション

1. Issueで議論
2. フォーク & ブランチ作成
3. 変更をコミット
4. プルリクエストを送信

## 📄 ライセンス

MIT License

## 🙏 謝辞

- WooCommerce チーム
- DeepL
- コントリビューターの皆様
