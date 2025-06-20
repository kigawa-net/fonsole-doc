# 共通データモデル

このドキュメントは、fomageおよびfonsoleで共通利用するデータモデルを定義します。

## User

ユーザー情報を表すモデルです。

| フィールド名 | 型 | 説明 |
|-----------|----|------|
| id | string | 一意のユーザーID |
| name | string | ユーザー名 |
| email | string | メールアドレス |
| createdAt | datetime | 作成日時 |
| updatedAt | datetime | 更新日時 |

**JSON表現**
```json
{
  "id": "507f1f77bcf86cd799439011",
  "name": "John Doe",
  "email": "john@example.com",
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-01-01T00:00:00Z"
}
```

## Data

汎用的なデータを表すモデルです。

| フィールド名 | 型 | 説明 |
|-----------|----|------|
| id | string | 一意のデータID |
| type | string | データタイプ（例: "document", "image"） |
| content | any | データの内容 |
| userId | string | 所有者ユーザーのID |
| createdAt | datetime | 作成日時 |
| updatedAt | datetime | 更新日時 |

**JSON表現**
```json
{
  "id": "507f1f77bcf86cd799439012",
  "type": "document",
  "content": "Sample content",
  "userId": "507f1f77bcf86cd799439011",
  "createdAt": "2024-01-01T00:00:00Z"
}
```

## Pagination

ページネーション情報を提供するためのモデルです。

| フィールド名 | 型 | 説明 |
|-----------|----|------|
| page | integer | 現在のページ番号 |
| size | integer | 1ページあたりのアイテム数 |
| total | integer | 総アイテム数 |
| totalPages | integer | 総ページ数 |

**JSON表現**
```json
{
  "page": 1,
  "size": 20,
  "total": 100,
  "totalPages": 5
}
```

---

今後、必要に応じて他の共通モデルを追加してください。 