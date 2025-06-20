# fonsole

## 概要

`fonsole` は、ファイルやディレクトリをMongoDBにバックアップおよびリストアするためのコマンドラインツールです。
定期的にバックアップを実行したり、特定の時点のバックアップからファイルを復元したりするのに役立ちます。

## 機能

-   指定したディレクトリのバックアップ
-   バックアップからのリストア
-   古いバックアップの自動削除

## ドキュメント

より詳細な情報については、以下のドキュメントを参照してください。

-   [**クイックスタート**](./QUICKSTART.md): `fonsole`の基本的な使い方と設定方法について説明します。
-   [**API (MongoDB スキーマ)**](./API.md): `fonsole`が使用するMongoDBのコレクションとスキーマについて説明します。

## 使い方

`fonsole` はDockerイメージとして提供されています。`docker run` コマンドで簡単に実行できます。

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

## MongoDB スキーマ

`fonsole`はMongoDBにデータを保存します。使用されるコレクションとスキーマは以下の通りです。

### `projects` コレクション

プロジェクトに関する情報を格納します。

| フィールド名 | 型 | 説明 |
| :--- | :--- | :--- |
| `_id` | `ObjectId` | ドキュメントの一意なID |
| `name` | `String` | プロジェクト名 (`PROJECT_NAME` 環境変数で指定) |
| `backupIds` | `Array<ObjectId>` | このプロジェクトに属するバックアップドキュメントのIDリスト |

**ドキュメント例:**
```json
{
  "_id": "653a1234567890abcdef12345",
  "name": "my-project",
  "backupIds": [
    "653b67890abcdef1234567890",
    "653c0123456789abcdef12345"
  ]
}
```

### `backups` コレクション

各バックアップ処理の情報を格納します。

| フィールド名 | 型 | 説明 |
| :--- | :--- | :--- |
| `_id` | `ObjectId` | ドキュメントの一意なID |
| `startDate` | `Date` | バックアップ処理の開始日時 |
| `rootDirectory` | `Object` | バックアップ対象のディレクトリ構造 |
| `endDate` | `Date` | バックアップ処理の終了日時 |
| `removeRequest` | `Boolean` | このバックアップが削除対象としてマークされているか |

`rootDirectory` オブジェクトは再帰的な構造で、ディレクトリとファイルの情報を含みます。

**ドキュメント例:**
```json
{
  "_id": "653b67890abcdef1234567890",
  "startDate": "2023-10-27T10:00:00Z",
  "rootDirectory": {
    "name": "backup",
    "files": [
      {
        "name": "file1.txt",
        "fileId": "653d4567890abcdef12345678"
      }
    ],
    "directories": [
      {
        "name": "subdir",
        "files": [],
        "directories": []
      }
    ]
  },
  "endDate": "2023-10-27T10:05:00Z",
  "removeRequest": false
}
```

### GridFS コレクション (`fs.files` と `fs.chunks`)

ファイルのバックアップにはGridFSが使用されます。

- **`fs.files`**: 各ファイルに関するメタデータ（ファイル名、サイズ、アップロード日など）を格納します。`backups` コレクションの `fileId` は、このコレクションのドキュメントIDを参照します。
- **`fs.chunks`**: ファイルの実際のコンテンツを小さなチャンクに分割して格納します。
