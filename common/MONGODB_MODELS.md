# MongoDB ドキュメントモデル（共通）

このドキュメントは、fonsoleで利用されるMongoDBコレクションのデータモデルを定義します。

## projects コレクション

プロジェクトごとの情報を管理します。

### ドキュメント例
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

| フィールド名   | 型             | 説明                                         |
|:--------------|:---------------|:---------------------------------------------|
| `_id`         | ObjectId        | ドキュメントの一意なID                      |
| `name`        | String          | プロジェクト名（環境変数`PROJECT_NAME`）     |
| `backupIds`   | Array<ObjectId> | このプロジェクトに属するバックアップIDリスト |

---

## backups コレクション

各バックアップ処理の情報を管理します。

### ドキュメント例
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

| フィールド名     | 型         | 説明                                         |
|:----------------|:-----------|:---------------------------------------------|
| `_id`           | ObjectId   | ドキュメントの一意なID                      |
| `startDate`     | Date       | バックアップ処理の開始日時                   |
| `rootDirectory` | Object     | バックアップ対象のディレクトリ構造           |
| `endDate`       | Date       | バックアップ処理の終了日時                   |
| `removeRequest` | Boolean    | 削除対象としてマークされているか             |

---

## GridFS（fs.files, fs.chunks）

バックアップファイル本体はGridFS（`fs.files`と`fs.chunks`）に保存されます。

- `fs.files`にはファイルのメタデータ、`fs.chunks`にはファイル本体の分割データが格納されます。

### fs.files ドキュメント例
```json
{
  "_id": "653d4567890abcdef12345678",
  "filename": "file1.txt",
  "length": 12345,
  "uploadDate": "2023-10-27T10:00:01Z"
  // ...その他GridFSの標準フィールド
}
```

---

今後、他のコレクションやフィールドが追加された場合はこのドキュメントに追記してください。 