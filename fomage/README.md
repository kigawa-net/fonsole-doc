# Fomage ドキュメント

これは、`fonsole`プロジェクトのMongoDB管理ツールである`fomage`の公式ドキュメントです。

## 🌟 はじめに

`fomage`は、`fonsole`アプリケーションのバックアップデータを格納するMongoDBデータベースを、効率的に管理・運用するために設計されたWebベースの管理ツールです。マルチモジュール構成として実装され、Web UIとREST APIが分離されています。

主な機能：
- **データ閲覧と編集**: `fonsole`が作成したバックアップデータやプロジェクト情報をGUIで直感的に操作できます。
- **スキーマ管理**: `fonsole`のコレクションスキーマを動的に取得し、表示します。
- **運用タスクの自動化**: 定期的なデータクリーンアップやインデックス作成などの管理タスクをサポートします。
- **監視とアラート**: データベースのパフォーマンスメトリクスを監視し、異常を検知します。

このドキュメントは、`fomage`のセットアップから開発、運用に至るまで、必要なすべての情報を提供します。

## 📚 ドキュメント一覧

### 🚀 スタートガイド

- **[クイックスタートガイド](QUICKSTART.md)** - 5分で始める`fomage`のセットアップ
- **[fonsoleとの連携](INTEGRATION.md)** - `fonsole`と`fomage`を連携させるための詳細ガイド

### 👨‍💻 開発者向け

- **[開発ガイド](DEVELOPMENT.md)** - 詳細な開発環境セットアップとコーディング規約
- **[環境変数設定](CONFIGURATION.md)** - アプリケーションの設定方法
- **[APIドキュメント](API_OVERVIEW.md)** - APIの概要、認証、共通仕様
- **[エンドポイント](ENDPOINTS.md)** - APIエンドポイント一覧
- **[データモデル](DATA_MODELS.md)** - APIのデータモデル定義

### 🏗️ アーキテクチャ

- **[システム設計](ARCHITECTURE.md)** - `fomage`のアーキテクチャと技術スタック
- **[データベース設計](DATABASE.md)** - `fomage`が使用するデータベースのスキーマ

### 🚢 デプロイと運用

- **[Kubernetesガイド](KUBERNETES.md)** - コンテナ化とデプロイメント手順
- **[本番環境セットアップ](PRODUCTION.md)** - 本番環境での`fomage`運用ガイド
- **[環境変数設定](CONFIGURATION.md)** - 運用環境での設定値
- **[監視とログ](MONITORING.md)** - GrafanaとPrometheusによる監視
- **[トラブルシューティング](TROUBLESHOOTING.md)** - よくある問題と解決策

### 🔒 セキュリティ

- **[セキュリティガイド](SECURITY.md)** - セキュリティのベストプラクティス
- **[認証・認可](AUTHENTICATION.md)** - `fomage`のユーザー認証と権限管理

## 🎯 ドキュメントの使い方

### 初めて利用する方

1.  **[クイックスタートガイド](QUICKSTART.md)** を読み、`fomage`を動かしてみましょう。
2.  **[fonsoleとの連携](INTEGRATION.md)** を参考に、既存の`fonsole`プロジェクトに接続します。

### 開発者の方

1.  **[開発ガイド](DEVELOPMENT.md)** で開発環境をセットアップしてください。
2.  **[環境変数設定](CONFIGURATION.md)** で必要な設定を行ってください。
3.  **[APIドキュメント](API_OVERVIEW.md)** でAPIの全体像を把握してください。
4.  必要に応じて **[エンドポイント](ENDPOINTS.md)** や **[データモデル](DATA_MODELS.md)** を参照してください。
5.  **[システム設計](ARCHITECTURE.md)** で`fomage`の内部構造を理解してください。

### 運用担当者の方

1.  **[Kubernetesガイド](KUBERNETES.md)** でデプロイ方法を確認してください。
2.  **[本番環境セットアップ](PRODUCTION.md)** で本番環境を構築してください。
3.  **[環境変数設定](CONFIGURATION.md)** で本番用の設定を行ってください。
4.  **[監視とログ](MONITORING.md)** で運用監視を設定してください。

## 📋 ドキュメントの更新

### ドキュメントの改善

ドキュメントの改善提案や修正は、以下の方法で行ってください：

1. GitHub Issuesで問題を報告
2. プルリクエストで修正を提案
3. 直接編集してコミット

### ドキュメントの追加

新しいドキュメントを追加する場合：

1. 適切なディレクトリにファイルを作成
2. このREADME.mdにリンクを追加
3. 必要に応じて目次を更新

## 🔍 ドキュメント検索

### キーワード検索

| キーワード | 関連ドキュメント |
|-----------|-----------------|
| セットアップ | [クイックスタートガイド](QUICKSTART.md) |
| 連携 | [fonsoleとの連携](INTEGRATION.md) |
| 開発環境 | [開発ガイド](DEVELOPMENT.md) |
| 設定 | [環境変数設定](CONFIGURATION.md) |
| API | [APIドキュメント](API_OVERVIEW.md) |
| エンドポイント | [エンドポイント](ENDPOINTS.md) |
| Kubernetes | [Kubernetesガイド](KUBERNETES.md) |
| デプロイ | [本番環境セットアップ](PRODUCTION.md) |
| 監視 | [監視とログ](MONITORING.md) |
| セキュリティ | [セキュリティガイド](SECURITY.md) |
| トラブル | [トラブルシューティング](TROUBLESHOOTING.md) |

## 📞 サポート

### ドキュメントに関する問題

- **不正確な情報**: GitHub Issuesで報告してください
- **不足している情報**: プルリクエストで追加を提案してください
- **翻訳**: 多言語対応の提案も歓迎します

### 技術的な問題

- **バグ報告**: GitHub Issuesで詳細を報告してください
- **機能要求**: GitHub Issuesで要望を提出してください
- **セキュリティ問題**: プライベートで報告してください

## 📝 ドキュメントの品質

### 品質基準

- **正確性**: 技術的に正確な情報を提供
- **完全性**: 必要な情報を漏れなく記載
- **明確性**: 分かりやすい説明と例を提供
- **最新性**: 定期的に更新して最新の状態を維持

### レビュープロセス

1. **技術レビュー**: 技術的な正確性を確認
2. **編集レビュー**: 文章の品質と分かりやすさを確認
3. **ユーザビリティテスト**: 実際のユーザーが理解できるかを確認

## 🔄 更新履歴

| 日付 | 更新内容 | 更新者 |
|------|----------|--------|
| 2024-01-01 | 初回作成 | 開発チーム |
| 2024-01-01 | クイックスタートガイド追加 | 開発チーム |
| 2024-01-01 | APIドキュメント追加 | 開発チーム |
| 2024-01-02 | Fomage概要とfonsole連携の説明を追加 | AIアシスタント |

---

**注意**: このドキュメントは継続的に更新されます。最新の情報を確認するために、定期的にチェックしてください。

## アーキテクチャ

`fomage`は以下の2つのモジュールで構成されています：

- **fomage** (Web UI): Thymeleafテンプレートエンジンを使用したWebインターフェース
- **fomage-api** (REST API): MongoDBと連携するRESTful API

## 技術スタック

- **Spring Boot 3.2.0**: アプリケーションフレームワーク
- **Kotlin 1.9.20**: プログラミング言語
- **Spring MVC**: Webフレームワーク
- **Thymeleaf**: テンプレートエンジン
- **Spring Data MongoDB**: データアクセス
- **Spring Security**: セキュリティ
- **WebClient**: HTTPクライアント
- **Gradle**: ビルドツール

## プロジェクト構造

```
fomage/
├── build.gradle                    # ルートプロジェクトのビルド設定
├── settings.gradle                 # マルチモジュール設定
├── fomage/                         # Web UI モジュール
│   ├── build.gradle               # Web UI モジュールのビルド設定
│   └── src/
│       ├── main/
│       │   ├── kotlin/net/kigawa/fomage/
│       │   │   ├── FomageWebApplication.kt    # Web UI メインクラス
│       │   │   ├── controller/                # Web Layer
│       │   │   ├── service/                   # Web Service Layer
│       │   │   ├── model/                     # UI用データモデル
│       │   │   └── config/                    # 設定クラス
│       │   └── resources/
│       │       ├── templates/                 # Thymeleafテンプレート
│       │       ├── static/                    # 静的リソース
│       │       └── application.yml           # Web UI設定
│       └── test/                              # テストコード
└── fomage-api/                    # REST API モジュール
    ├── build.gradle              # API モジュールのビルド設定
    └── src/
        ├── main/
        │   ├── kotlin/net/kigawa/fomage/api/
        │   │   ├── FomageApiApplication.kt    # API メインクラス
        │   │   ├── controller/                # REST API Layer
        │   │   ├── service/                   # API Service Layer
        │   │   ├── repository/                # Data Layer
        │   │   ├── model/                     # API用データモデル
        │   │   └── config/                    # 設定クラス
        │   └── resources/
        │       └── application.yml            # API設定
        └── test/                               # テストコード
```

## セットアップ

### 前提条件

- Java 17以上
- Gradle 7.0以上
- MongoDB 4.4以上

### インストール

1. リポジトリをクローン
```bash
git clone <repository-url>
cd fomage
```

2. プロジェクトをビルド
```bash
./gradlew build
```

3. MongoDBを起動
```bash
# MongoDBがインストールされている場合
mongod

# Dockerを使用する場合
docker run -d -p 27017:27017 --name mongodb mongo:4.4
```

## 実行

### 開発環境での実行

1. REST APIを起動
```bash
./gradlew :fomage-api:bootRun
```

2. Web UIを起動（別のターミナルで）
```bash
./gradlew :fomage:bootRun
```

### 本番環境での実行

1. JARファイルをビルド
```bash
./gradlew :fomage-api:bootJar
./gradlew :fomage:bootJar
```

2. アプリケーションを起動
```bash
# API
java -jar fomage-api/build/libs/fomage-api-0.0.1-SNAPSHOT.jar

# Web UI
java -jar fomage/build/libs/fomage-0.0.1-SNAPSHOT.jar
```

## 設定

### Web UI設定 (fomage/src/main/resources/application.yml)

```yaml
server:
  port: 8080

fomage:
  api:
    base-url: http://localhost:8081
    timeout: 5000
```

### REST API設定 (fomage-api/src/main/resources/application.yml)

```yaml
server:
  port: 8081

spring:
  data:
    mongodb:
      uri: mongodb://localhost:27017/fonsole
      database: fonsole
```

## API仕様

### データ管理API

- `GET /api/v1/data` - データ一覧取得
- `GET /api/v1/data/{id}` - データ詳細取得
- `POST /api/v1/data` - データ作成
- `PUT /api/v1/data/{id}` - データ更新
- `DELETE /api/v1/data/{id}` - データ削除
- `GET /api/v1/data/projects` - プロジェクト一覧取得
- `GET /api/v1/data/types` - データタイプ一覧取得

## 開発

### モジュールの追加

新しいモジュールを追加する場合：

1. `settings.gradle`にモジュールを追加
```gradle
include 'new-module'
```

2. 新しいモジュールの`build.gradle`を作成
3. 必要な依存関係を設定

### テスト

```bash
# 全モジュールのテストを実行
./gradlew test

# 特定のモジュールのテストを実行
./gradlew :fomage:test
./gradlew :fomage-api:test
```

## デプロイ

### Docker

各モジュール用のDockerfileを作成し、個別にコンテナ化できます。

### Kubernetes

各モジュールを個別のDeploymentとしてデプロイし、Serviceで連携させます。

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。 