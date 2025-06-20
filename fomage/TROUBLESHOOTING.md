# トラブルシューティング

このガイドは、`fomage`のセットアップや運用中に発生する可能性のある一般的な問題とその解決策をまとめたものです。

## よくある問題

### 1. ポートが既に使用されている

**症状**: `docker-compose up` や `./gradlew run` を実行した際に、ポートが既に使用されているというエラーメッセージが表示される。

**解決策**:
1.  **使用中のポートを確認**:
    ```bash
    # 8080ポートを使用しているプロセスを検索
    lsof -i :8080
    ```
2.  **別のポートを使用**:
    `.env` ファイルで `SERVER_PORT` の値を変更します。
    ```dotenv
    # .env
    SERVER_PORT=8082
    ```

### 2. MongoDB接続エラー

**症状**: アプリケーションログにMongoDBへの接続失敗に関するエラーが表示される。

**解決策**:
1.  **MongoDBの状態を確認**:
    Docker Composeで起動している場合、コンテナが正常に動作しているか確認します。
    ```bash
    docker-compose ps mongodb
    ```
    `State`が `Up` になっていることを確認してください。

2.  **MongoDBログを確認**:
    コンテナのログにエラーが出ていないか確認します。
    ```bash
    docker-compose logs mongodb
    ```

3.  **接続設定を確認**:
    `.env` ファイルの `MONGODB_URI` が正しいか（ホスト名、ポート、認証情報など）確認してください。

### 3. メモリ不足エラー (OutOfMemoryError)

**症状**: アプリケーションが `java.lang.OutOfMemoryError` でクラッシュする。

**解決策**:
JVMのヒープサイズを増やします。Gradleで実行する場合、環境変数 `JAVA_OPTS` を設定します。
```bash
# JVMヒープサイズを初期1GB、最大2GBに増加
JAVA_OPTS="-Xms1g -Xmx2g" ./gradlew run
```

### 4. Dockerイメージのビルドエラー

**症状**: `docker-compose build` が失敗する。

**解決策**:
1.  **Dockerキャッシュをクリア**:
    古いキャッシュが原因の場合があります。システム全体のキャッシュを削除して再試行します。
    ```bash
    docker system prune -a
    ```
2.  **イメージを再ビルド**:
    キャッシュを使わずにイメージを強制的に再ビルドします。
    ```bash
    docker-compose build --no-cache
    ```

## ログレベルの変更

より詳細なデバッグ情報が必要な場合は、ログレベルを変更します。

```bash
# .envファイルでLOG_LEVELをDEBUGに設定
LOG_LEVEL=DEBUG
```

その後、アプリケーションを再起動してログを確認してください。 