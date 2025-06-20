# クイックスタートガイド

このガイドでは、`fomage`を5分でセットアップし、`fonsole`のバックアップデータを管理し始めるための手順を説明します。推奨される `Docker Compose` を利用した方法で進めます。

## 1. 前提条件

ローカル環境に以下のツールがインストールされていることを確認してください。

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## 2. セットアップ

### ステップ1: リポジトリをクローン

まず、`fomage`のリポジトリをローカルにクローンします。

```bash
git clone https://github.com/kigawa-net/fonsole-doc.git
cd fonsole-doc
```


### ステップ2: `.env` ファイルの作成

`fomage`の動作に必要な設定を行うため、プロジェクトのルートディレクトリに `.env` ファイルを作成します。
以下は、ローカルの `fonsole` データベースに接続するための設定例です。

```bash
# .env ファイルを作成して、以下の内容をコピー＆ペーストしてください。
touch .env
```

**.env ファイルの内容:**
```dotenv
# Fomage アプリケーション設定
FOMAGE_SERVER_PORT=8080

# Fonsole の MongoDB データベース接続設定
# 接続先のURIを環境に合わせて変更してください
MONGODB_URI=mongodb://admin:password123@localhost:27017/fonsole-backup?authSource=admin

# ログレベル (DEBUG, INFO, WARN, ERROR)
LOG_LEVEL=INFO
```

### ステップ3: アプリケーションの起動

`docker-compose` を使って、`fomage` と関連サービス（MongoDBなど）を一度に起動します。

```bash
docker-compose up -d
```

起動には数分かかる場合があります。

## 3. 動作確認

サービスが正常に起動したか確認しましょう。

### Fomage管理パネルへのアクセス

Webブラウザで [http://localhost:8080](http://localhost:8080) を開きます。
`fomage` のダッシュボードが表示されれば成功です。`fonsole`のプロジェクトデータが表示されていることを確認してください。

### ログの確認

何か問題が発生した場合や、動作状況を確認したい場合は、以下のコマンドでログを参照できます。

```bash
docker-compose logs -f
```

## 4. アプリケーションの停止

`fomage`を停止するには、以下のコマンドを実行します。

```bash
docker-compose down
```
`-v` オプションを付けると、Dockerボリューム（データベースのデータなど）も一緒に削除されます。
```bash
docker-compose down -v
```

## 📚 次のステップ

より詳細な情報については、以下のドキュメントを参照してください。

1.  **[設定ガイド](CONFIGURATION.md)**: 環境変数や設定の詳細について説明します。
2.  **[開発ガイド](DEVELOPMENT.md)**: ローカルでの開発環境構築、テスト、デバッグ、開発ワークフローについて説明します。
3.  **[監視ガイド](MONITORING.md)**: ログの確認方法やヘルスチェックについて説明します。
4.  **[トラブルシューティング](TROUBLESHOOTING.md)**: よくある問題とその解決策をまとめています。
5.  **[APIドキュメント](API_OVERVIEW.md)**: APIの仕様について説明します。
6.  **[アーキテクチャ](ARCHITECTURE.md)**: システム全体の設計について説明します。

## 🆘 サポート

問題が発生した場合：

1.  **[トラブルシューティング](TROUBLESHOOTING.md)** セクションを確認
2.  **[GitHub Issues](https://github.com/your-repo/issues)** で既存の問題を検索
3.  新しいIssueを作成

## 📝 変更履歴

| バージョン | 日付       | 変更内容     |
|-----------|------------|--------------|
| 1.0.0     | 2024-01-01 | 初回リリース |

---

**注意**: 本番環境で使用する前に、必ずセキュリティ設定を確認し、適切な値に変更してください。 