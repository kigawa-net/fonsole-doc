# MongoDB スキーマ

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