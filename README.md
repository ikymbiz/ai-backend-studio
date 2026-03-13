# AI Backend Studio

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-Web-lightgrey.svg)
![AI](https://img.shields.io/badge/AI-Gemini_API-orange.svg)

## 概要・説明
**AI Backend Studio** は、ブラウザ上で完結するAI駆動のバックエンドAPI開発・デプロイメント環境（SPA）です。

自然言語で「どんなAPIを作りたいか」をAI（Gemini API）に伝えるだけで、必要なPythonファイル、要件定義、Dockerfileなどを一括生成します。生成されたコードはブラウザ上のエディタで即座に編集でき、内蔵のテストツールでAPIの動作確認が可能です。さらに、GitHubへのコードプッシュと、GitHub Actionsを経由したGoogle Cloud RunへのCI/CDデプロイまで、すべてこのアプリ単体でシームレスに完結します。

モバイル端末での表示・操作にも最適化されており、いつでもどこでもバックエンド開発からインフラ構築までを行うことができます。

## 主な機能一覧

- 💬 **AI チャット駆動開発 (CHAT)**
  - プロンプトを入力するだけで、Gemini APIがプロジェクトに必要な複数ファイル（`main.py`, `requirements.txt`, `Dockerfile` 等）を文脈を理解して自動生成します。
- 💻 **コードエディタ (CODE)**
  - シンタックスハイライト付きの本格的なエディタ（CodeMirror）を搭載。自動保存機能により、作業のやり直しを防ぎます。
- 📁 **ファイルシステム (FILES)**
  - IndexedDBを用いた仮想ファイルシステム。ファイルの新規作成・削除、プロジェクト全体のJSONエクスポート/インポート、既存のGitHubリポジトリからのファイルインポートが可能です。
- 🧪 **APIテスター (TEST)**
  - PostmanライクなHTTPリクエスト送信ツールを内蔵。メソッド、URL、ヘッダー、ボディ（JSON）を指定して、開発したAPIの挙動を即座にテストできます。
- 🚀 **ワンクリック・デプロイ (DEPLOY)**
  - GitHub PATを利用して、指定したリポジトリへコードを一括プッシュ。
  - GitHub Actionsのワークフローファイル（`deploy.yml`）を自動生成し、Google Cloud Runへのデプロイを完全自動化。GitHub Pagesの有効化もトグル一つで設定可能です。
- ☁️ **インフラ管理 (INFRA)**
  - Firestoreのコレクション・ドキュメントの仮想モックエクスプローラー。
  - コレクション構成からFirestoreのセキュリティルールをAIで自動生成する機能を搭載。環境変数の管理も可能です。
- ⚙️ **カスタマイズ (SETTINGS)**
  - 利用するAIモデルの切り替え（Gemini 2.5 Flash / Pro 等）、各種APIキー、GCPプロジェクトIDなどを安全にローカル保存します。

## 使用技術・ライブラリ

本プロジェクトはビルドツールを必要としないピュアなHTML/JS/CSS構成で、モダンなライブラリをCDN経由で活用しています。

**フロントエンド・UI**
- HTML5 / CSS3 / Vanilla JavaScript
- **Tailwind CSS** (スタイリング)
- **FontAwesome** (アイコン)

**エディタ・マークダウン解析**
- **CodeMirror 5** (コードエディタ / Python, YAML, Dockerfile, JS, Markdown対応)
- **Marked.js** (MarkdownのHTML変換)
- **DOMPurify** (サニタイズによるXSS対策)

**データ保存**
- **Dexie.js** (IndexedDBラッパー / オフラインでのファイル・履歴・設定保存)

**外部API・連携サービス**
- **Google Gemini API** (AIによるコード生成・解析)
- **GitHub REST API** (リポジトリ作成、ファイルプッシュ、インポート)
- **Google Cloud Run / Actions** (自動デプロイ先として想定)

## セットアップ・使い方

本アプリはサーバーレスで動作します。HTMLファイルをブラウザで開くだけで即座に利用を開始できます。

### 1. 事前準備
本アプリのフル機能を活用するために、以下の情報を取得・準備してください。
1. **Gemini API キー**: [Google AI Studio](https://aistudio.google.com/) から取得します。
2. **GitHub PAT (Personal Access Token)**: [GitHub Developer Settings](https://github.com/settings/tokens/new?scopes=repo,workflow) で `repo`, `workflow` 権限を付与して作成します。
3. **GCPの準備**: Google Cloudプロジェクトを作成し、`Cloud Run API` と `Cloud Build API` を有効化します。
4. **GCP 認証情報**: GitHub Actionsからデプロイするため、GCPのサービスアカウントキー（JSON）を発行し、デプロイ先GitHubリポジトリのSecretsに `GCP_CREDENTIALS` という名前で登録しておきます。

### 2. 初期設定
1. 本HTMLファイルをブラウザ（Chrome推奨）で開きます。
2. 下部ナビゲーションから **CFG (Settings)** タブを開きます。
3. 取得した「Gemini API Key」「GitHub PAT」「GCP Project ID」「Cloud Run Service 名」を入力し、**Save** ボタンを押してローカルに保存します。

### 3. アプリケーションの開発とデプロイ
1. **開発**: **CHAT** タブで「FastAPIでJWT認証付きのログインAPIを作って」と入力し送信します。
2. **確認・編集**: AIがコードを生成し終わると、**FILES** タブにファイル一覧が表示されます。**CODE** タブで内容を確認・修正します。
3. **デプロイ**: **DEPLOY** タブを開き、作成したいGitHubリポジトリ名を入力します。CI/CD WorkflowのトグルをONにして **Push & Deploy** をクリックします。
4. **テスト**: デプロイが完了し、Cloud RunのURLが発行されたら、**TEST** タブでそのURLに向けてリクエストを送信し、正しく動作するか確認します。

---
*Note: 本アプリに保存されたコードやAPIキーなどのデータは、すべてブラウザのローカルストレージ（IndexedDB）内にのみ安全に保存されます。*