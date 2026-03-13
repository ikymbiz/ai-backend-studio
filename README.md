# AI Backend Studio

AI Backend Studioは、ブラウザ上で完結する**AI駆動型のバックエンド開発・デプロイ環境**です。
Google Gemini APIを活用して、自然言語のプロンプトからAPIコード（FastAPIやCloudflare Workersなど）を自動生成し、ブラウザ内蔵のエディタで編集、さらにはGCP Cloud RunやCloudflare Workersへのデプロイまでをシームレスに行うことができます。

## 🌟 概要・主な特徴

- **オールインワンのWeb IDE**: チャット、コードエディタ、ファイル管理、APIテスト、デプロイ、インフラ管理を一つの画面に統合。
- **AIアシスタント内蔵**: 要件を伝えるだけで、AIがプロジェクトに必要な複数ファイル（Pythonスクリプト、JS/TS、Dockerfile、デプロイ設定ファイルなど）を一括生成します。
- **ブラウザ完結のローカル保存**: Dexie.js（IndexedDB）を使用し、すべてのプロジェクトファイルや設定、チャット履歴はブラウザに安全に保存されます。
- **ワンクリック・デプロイ**: 
  - **GCP Cloud Run**: GitHub API経由でリポジトリを作成・プッシュし、GitHub Actions（CI/CD）を利用して自動デプロイを行います。
  - **Cloudflare Workers**: Cloudflare APIを叩き、ブラウザから直接エッジ環境へWorkerスクリプトをデプロイします。

## 🚀 主な機能一覧

### 1. 💬 CHAT (AIチャットアシスタント)
- Gemini API（Web Worker上で非同期動作）を利用した高速なコード生成。
- ユーザーのプロンプトと既存のプロジェクトファイルをコンテキストとしてAIに渡し、精度の高いコード改修を実現。

### 2. 📝 CODE & FILES (エディタとファイル管理)
- **CodeMirror 5**によるシンタックスハイライト付きエディタ（Python, JS/TS, YAML, TOML, Dockerfile, Markdown対応）。
- 仮想ファイルシステムでの新規作成、削除、ブラウザ内オートセーブ。
- プロジェクト全体のJSONエクスポート/インポート機能。
- GitHubリポジトリからのファイル直接インポート機能。

### 3. 🧪 TEST (APIテスター)
- Postmanライクな組み込みHTTPクライアント。
- メソッド（GET, POST, PUT, DELETE, PATCH）、URL、JSONヘッダー・ボディを指定してAPIの動作検証が可能。

### 4. ☁️ DEPLOY (デプロイ機能)
- **GCPタブ**: GitHubリポジトリへのプッシュから、`deploy.yml` の自動生成を通じたCloud RunへのCI/CDパイプライン構築。
- **WORKERタブ**: ローカルのJS/TSファイルをCloudflare Workersへ直接アップロード＆デプロイ。

### 5. 🛠 INFRA (インフラ・環境変数管理)
- **Firestore Explorer**: 仮想的なFirestoreのコレクション・ドキュメント（JSON）エディタ。
- **Security Rules**: Firestoreのセキュリティルールを管理。AIによるルール自動生成機能付き。
- GCPおよびCloudflare向けの環境変数（Environment Variables）の追加・管理機能。

### 6. ⚙️ CFG (設定)
- Gemini APIキー、AIモデルの選択（Flash/Pro等）。
- GitHub PAT（Personal Access Token）設定。
- GCP Project ID、Cloud Runサービス名の設定。
- Cloudflare Account ID、API Token、Worker名の設定。

## 🛠 使用技術・ライブラリ

- **フロントエンド**: HTML5, CSS3, JavaScript (Vanilla)
- **スタイリング**: Tailwind CSS (CDN)
- **アイコン**: FontAwesome 6
- **エディタ**: CodeMirror 5
- **データベース**: Dexie.js (IndexedDB)
- **Markdown処理**: marked.js, DOMPurify
- **AI連携**: Google Gemini API (v1beta)
- **その他API**: GitHub REST API, Cloudflare API

## 📦 セットアップ・使い方

このアプリケーションはバックエンドサーバーを必要としません。単一のHTMLファイルとして動作します。

### ステップ 1: アプリの起動
1. 本リポジトリの `index.html` をダウンロードします。
2. ブラウザ（Chrome, Edge, Firefox, Safari等）で直接ファイルを開きます。
   *※ クリップボードAPIや一部の機能を利用するため、ローカルサーバー（VS CodeのLive Serverなど）経由での起動を推奨します。*

### ステップ 2: 初期設定
1. アプリ画面下部のナビゲーションから **CFG (⚙️)** タブを開きます。
2. [Google AI Studio](https://aistudio.google.com/) で取得した **Gemini API Key** を入力します。
3. （オプション）デプロイ機能を利用する場合は、以下の情報を入力して `Save` ボタンを押します。
   - **GCP用**: GitHub PAT、GCP Project ID
   - **Cloudflare用**: CF Account ID、CF API Token、Worker Name

### ステップ 3: 開発フロー
1. **コード生成**: **CHAT** タブで「Cloudflare WorkerでJSONを返すAPIを作って」と入力し、送信ボタンを押します。
2. **コード確認**: **FILES** タブに生成されたファイル（`index.js` 等）が表示されます。タップして **CODE** タブで内容を確認・編集します。
3. **デプロイ**: 
   - Cloudflareの場合は **WORKER** タブから `Deploy to CF Worker` をクリックします。
   - GCPの場合は **GCP** タブからリポジトリ名を入力し `Push & Deploy` をクリックします。
4. **テスト検証**: 発行されたエンドポイントURLを **TEST** タブに入力し、リクエストを送信して動作を確認します。

---

**⚠️ 注意事項**
- 本ツールで扱うAPIキーやトークンは、すべてブラウザ内のIndexedDBにのみ保存され、外部サーバーには送信されません（各公式APIの直接呼び出しを除く）。
- セキュリティ保護のため、他人がアクセスできる共有PCでの利用後は、CFGタブ最下部の「全データ削除」を実行してください。