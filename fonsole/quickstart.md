# クイックスタート

`fonsole` はDockerイメージとして提供されています。`docker run` コマンドで簡単に実行できます。

## 使い方

### バックアップ

以下のコマンドで、指定したディレクトリをMongoDBにバックアップします。

```bash
docker run --rm -it \
  -v /path/to/your/data:/backup \
  -e MONGO_USERNAME=<your_mongo_user> \
  -e MONGO_PASSWORD=<your_mongo_password> \
  -e MONGO_HOST=<your_mongo_host> \
  -e MONGO_PORT=<your_mongo_port> \
  -e MONGO_DATABASE_NAME=fonsole \
  -e PROJECT_NAME=<your_project_name> \
  -e BACKUP_PATH=/backup \
  kigawa01/fonsole backup
```

### リストア

以下のコマンドで、指定した日時のバックアップからファイルをリストアします。
リストア先のディレクトリは、実行時にマウントしたボリュームになります。

```bash
docker run --rm -it \
  -v /path/to/your/data:/backup \
  -e MONGO_USERNAME=<your_mongo_user> \
  -e MONGO_PASSWORD=<your_mongo_password> \
  -e MONGO_HOST=<your_mongo_host> \
  -e MONGO_PORT=<your_mongo_port> \
  -e MONGO_DATABASE_NAME=fonsole \
  -e PROJECT_NAME=<your_project_name> \
  -e BACKUP_PATH=/backup \
  -e "RESTORE_DATE=2023-10-27 10:00:00" \
  kigawa01/fonsole restore
```

## 設定

`fonsole` の動作は環境変数によって設定します。

### 必須の設定

| 環境変数名 | 説明 |
| :--- | :--- |
| `MONGO_USERNAME` | MongoDBのユーザー名 |
| `MONGO_PASSWORD` | MongoDBのパスワード |
| `MONGO_HOST` | MongoDBのホスト名 |
| `PROJECT_NAME` | バックアッププロジェクトを識別するための名前 |
| `BACKUP_PATH` | バックアップまたはリストア対象のディレクトリパス（コンテナ内のパス） |

### オプションの設定

| 環境変数名 | 説明 | デフォルト値 |
| :--- | :--- | :--- |
| `MONGO_PORT` | MongoDBのポート番号 | `27017` |
| `MONGO_DATABASE_NAME` | 使用するMongoDBのデータベース名 | `fonsole` |
| `MAX_REQUEST` | 同時に実行するリクエストの最大数 | `10` |
| `LOG_LEVEL` | アプリケーションのログレベル | `INFO` |
| `MONGO_LOG_LEVEL` | MongoDBドライバのログレベル | `INFO` |
| `RESTORE_DATE` | リストア時に使用。`yyyy-MM-dd HH:mm:ss` 形式で指定 | |

### `.env` ファイル

Dockerコマンドで環境変数を指定する代わりに、`.env` ファイルや `.env.local` ファイルに設定を記述することもできます。
実行するディレクトリにファイルを配置してください。

**`.env` ファイルの例:**
```
MONGO_USERNAME=user
MONGO_PASSWORD=password
MONGO_HOST=mongo
PROJECT_NAME=my-project
BACKUP_PATH=/backup
``` 