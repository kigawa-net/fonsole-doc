# API概要

このドキュメントは、FomageアプリケーションのAPIに関する共通仕様を説明します。

## 構成

Fomage APIのドキュメントは、以下の通り分割されています。

- **[API概要](API_OVERVIEW.md)** (このドキュメント)
  - APIの基本情報、認証、エラーハンドリングなど。
- **[エンドポイント一覧](ENDPOINTS.md)**
  - 利用可能なすべてのAPIエンドポイントの詳細仕様。
- **[データモデル](DATA_MODELS.md)**
  - APIで利用されるデータ構造の定義。

## ベースURL

```
http://localhost:8080/api/v1
```

## レスポンス形式

すべてのAPIレスポンスはJSON形式で返されます。

```json
{
  "success": true,
  "data": {},
  "message": "Operation completed successfully",
  "timestamp": "2024-01-01T00:00:00Z"
}
```

## 認証

Fomage APIは、リクエストの認証に[Keycloak](https://www.keycloak.org/)を利用したOAuth 2.0アクセストークンを使用します。すべてのリクエストには、`Authorization`ヘッダーに有効なアクセストークンを含める必要があります。

**ヘッダー形式**

```
Authorization: Bearer <YOUR_ACCESS_TOKEN>
```

アクセストークンは、Keycloakの認証サーバーから取得します。一般的なOAuth 2.0のフロー（例: クライアントクレデンシャルズフロー、認可コードフロー）を利用してトークンを取得してください。

詳細な認証・認可のフローについては、`AUTHENTICATION.md`（現在準備中）を参照してください。

## エラーハンドリング

APIリクエストでエラーが発生した場合、レスポンスの`success`フィールドは`false`になり、`error`オブジェクトが含まれます。

**エラーレスポンスの例**
```json
{
  "success": false,
  "error": {
    "code": 404,
    "message": "Resource not found"
  },
  "timestamp": "2024-01-01T00:00:00Z"
}
```

### 主なHTTPステータスコード

| コード | 説明 |
|----|---|
| 200 | OK - リクエスト成功 |
| 201 | Created - リソース作成成功 |
| 400 | Bad Request - 不正なリクエスト |
| 401 | Unauthorized - 認証エラー |
| 403 | Forbidden - アクセス拒否 |
| 404 | Not Found - リソースが見つからない |
| 500 | Internal Server Error - サーバー内部エラー |

## レート制限

APIの乱用を防ぐため、1ユーザーあたりのリクエスト数にはレート制限が設けられています。デフォルトでは、1分あたり100リクエストまでです。

レート制限を超えた場合、HTTPステータスコード`429 Too Many Requests`が返されます。
```
HTTP/1.1 429 Too Many Requests
Retry-After: 60
```

## サンプルコード

以下は、APIを利用するための簡単なサンプルコードです。

### cURL

```bash
curl -X GET \
  http://localhost:8080/api/v1/users \
  -H 'Authorization: Bearer <YOUR_ACCESS_TOKEN>'
```

### JavaScript (fetch)

```javascript
const accessToken = '<YOUR_ACCESS_TOKEN>';
const url = 'http://localhost:8080/api/v1/users';

fetch(url, {
  headers: {
    'Authorization': `Bearer ${accessToken}`
  }
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```
