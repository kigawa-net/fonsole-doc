# 監視とログ

このドキュメントでは、`fomage`アプリケーションの監視とログ収集に関する方法を説明します。

## ログの確認

アプリケーションの動作状況を把握するためには、ログの確認が不可欠です。

### Docker Compose環境

`docker-compose`でアプリケーションを運用している場合、以下のコマンドでログをリアルタイムに表示できます。

```bash
# fomage-appサービスのログを表示
docker-compose logs -f fomage-app
```

特定のサービスのログだけを見たい場合は、サービス名を指定します（例: `mongodb`）。

### ローカル実行環境

`./gradlew run`などで直接実行している場合、ログは通常ファイルに出力されます。

```bash
# ログファイルの末尾を監視
tail -f logs/application.log
```
※ログの出力先は、ロギング設定（`logback.xml`など）によって異なる場合があります。

## データベース管理

開発環境では、データベースの状態を直接確認できると便利です。`docker-compose.yml`で`mongo-express`のような管理ツールがセットアップされている場合、Web UIからMongoDBを操作できます。

-   **MongoDB Express**: [http://localhost:8081](http://localhost:8081)
    -   デフォルトの認証情報: `admin` / `password123`

本番環境では、セキュリティで保護された適切なデータベース監視ツール（MongoDB Atlas, Ops Managerなど）を使用してください。

## アプリケーションのヘルスチェック

アプリケーションが正常に動作しているかを確認するためのエンドポイントです。

```bash
# ヘルスチェックエンドポイントにリクエストを送信
curl http://localhost:8080/health
```
正常であれば `{"status":"UP"}` のようなレスポンスが返ってきます。これはロードバランサーのヘルスチェックなどにも利用できます。

## アプリケーション情報

バージョンやビルド情報などを確認するためのエンドポイントです。

```bash
# アプリケーション情報エンドポイントにリクエストを送信
curl http://localhost:8080/api/v1/info
``` 