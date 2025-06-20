# エンドポイント一覧

このドキュメントでは、Fomage APIが提供するすべてのエンドポイントについて詳述します。

## ヘルスチェック

### GET /health

アプリケーションの健全性を確認します。

**レスポンス**

```json
{
  "status": "UP",
  "timestamp": "2024-01-01T00:00:00Z",
  "version": "1.0.0"
}
```

## ユーザー管理

### GET /users

ユーザー一覧を取得します。

**クエリパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| page | integer | 否 | ページ番号（デフォルト: 1） |
| size | integer | 否 | ページサイズ（デフォルト: 20） |
| sort | string | 否 | ソート項目（例: name,createdAt） |
| order | string | 否 | ソート順序（asc, desc） |

**レスポンス**
```json
{
  "success": true,
  "data": {
    "users": [
      {
        "id": "507f1f77bcf86cd799439011",
        "name": "John Doe",
        "email": "john@example.com",
        "createdAt": "2024-01-01T00:00:00Z",
        "updatedAt": "2024-01-01T00:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "size": 20,
      "total": 100,
      "totalPages": 5
    }
  }
}
```

### GET /users/{id}

指定されたIDのユーザーを取得します。

**パスパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| id | string | 是 | ユーザーID |

**レスポンス**
```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T00:00:00Z"
  }
}
```

### POST /users

新しいユーザーを作成します。

**リクエストボディ**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securepassword123"
}
```

**レスポンス**
```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T00:00:00Z"
  },
  "message": "User created successfully"
}
```

### PUT /users/{id}

指定されたIDのユーザーを更新します。

**パスパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| id | string | 是 | ユーザーID |

**リクエストボディ**

```json
{
  "name": "John Smith",
  "email": "johnsmith@example.com"
}
```

**レスポンス**
```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "name": "John Smith",
    "email": "johnsmith@example.com",
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T12:00:00Z"
  },
  "message": "User updated successfully"
}
```

### DELETE /users/{id}

指定されたIDのユーザーを削除します。

**パスパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| id | string | 是 | ユーザーID |

**レスポンス**
```json
{
  "success": true,
  "message": "User deleted successfully"
}
```

## データ管理

### GET /data

データ一覧を取得します。

**クエリパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| type | string | 否 | データタイプでフィルタ |
| userId | string | 否 | ユーザーIDでフィルタ |
| page | integer | 否 | ページ番号 |
| size | integer | 否 | ページサイズ |

**レスポンス**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "507f1f77bcf86cd799439012",
        "type": "document",
        "content": "Sample content",
        "userId": "507f1f77bcf86cd799439011",
        "createdAt": "2024-01-01T00:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "size": 20,
      "total": 50,
      "totalPages": 3
    }
  }
}
```

### GET /data/{id}

指定されたIDのデータを取得します。

**パスパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| id | string | 是 | データID |

**レスポンス**
```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439012",
    "type": "document",
    "content": "Sample content",
    "userId": "507f1f77bcf86cd799439011",
    "createdAt": "2024-01-01T00:00:00Z"
  }
}
```

### POST /data

新しいデータを作成します。

**リクエストボディ**

```json
{
  "type": "document",
  "content": "New content here",
  "userId": "507f1f77bcf86cd799439011"
}
```

**レスポンス**
```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439013",
    "type": "document",
    "content": "New content here",
    "userId": "507f1f77bcf86cd799439011",
    "createdAt": "2024-01-01T00:00:00Z"
  },
  "message": "Data created successfully"
}
```

### PUT /data/{id}

指定されたIDのデータを更新します。

**パスパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| id | string | 是 | データID |

**リクエストボディ**
```json
{
  "content": "Updated content"
}
```

**レスポンス**
```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439012",
    "type": "document",
    "content": "Updated content",
    "userId": "507f1f77bcf86cd799439011",
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T13:00:00Z"
  },
  "message": "Data updated successfully"
}
```

### DELETE /data/{id}

指定されたIDのデータを削除します。

**パスパラメータ**

| パラメータ | 型 | 必須 | 説明 |
|-----------|----|------|------|
| id | string | 是 | データID |

**レスポンス**
```json
{
  "success": true,
  "message": "Data deleted successfully"
}
```

## 設定管理

### GET /settings

現在の設定を取得します。

**レスポンス**
```json
{
  "success": true,
  "data": {
    "theme": "dark",
    "notifications": {
      "email": true,
      "sms": false
    }
  }
}
```

### PUT /settings

設定を更新します。

**リクエストボディ**
```json
{
  "theme": "light",
  "notifications": {
    "email": true,
    "sms": true
  }
}
```

**レスポンス**
```json
{
  "success": true,
  "data": {
    "theme": "light",
    "notifications": {
      "email": true,
      "sms": true
    }
  },
  "message": "Settings updated successfully"
}
```

## データ管理API

`fonsole`のバックアップデータを管理するための基本的なAPIです。

| メソッド | パス | 説明 |
|---|---|---|
| `GET` | `/api/v1/data` | データ一覧取得 |
| `GET` | `/api/v1/data/{id}` | データ詳細取得 |
| `POST` | `/api/v1/data` | データ作成 |
| `PUT` | `/api/v1/data/{id}` | データ更新 |
| `DELETE` | `/api/v1/data/{id}` | データ削除 |
| `GET` | `/api/v1/data/projects` | プロジェクト一覧取得 |
| `GET` | `/api/v1/data/types` | データタイプ一覧取得 | 