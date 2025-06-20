# 認証・認可

このドキュメントでは、Fomage APIのKeycloakを利用した認証・認可の仕組みについて詳しく説明します。

## 概要

Fomage APIは、セキュリティと柔軟性を確保するために、認証・認可の基盤として[Keycloak](https://www.keycloak.org/)を採用しています。すべてのAPIリクエストは、Keycloakによって発行された有効なアクセストークンによって保護される必要があります。

## 用語集

- **Keycloak**: オープンソースのアイデンティティおよびアクセス管理ソリューション。
- **レルム (Realm)**: ユーザー、クライアント、ロールなどを管理する独立した空間。Fomageは専用のレルムで運用されます。
- **クライアント (Client)**: Keycloakに保護されるアプリケーションやサービス（この場合はFomage API）。
- **ロール (Role)**: ユーザーやクライアントに割り当てられる権限。APIのアクセス制御に使用されます。
- **アクセストークン (Access Token)**: クライアントが保護されたリソース（APIエンドポイント）にアクセスするために使用する資格情報（JWT形式）。

## 認証フロー

Fomage APIでは、主に**クライアントクレデンシャルズフロー**を利用してアクセストークンを取得します。これは、サーバー間の通信など、ユーザーが介在しないM2M（Machine-to-Machine）のユースケースを想定しています。

### クライアントクレデンシャルズフロー

このフローでは、クライアントが自身のクライアントIDとクライアントシークレットを使って、Keycloakのトークンエンドポイントに直接リクエストを送信し、アクセストークンを取得します。

**トークン取得の例 (cURL)**

```bash
curl -X POST \
  "http://<KEYCLOAK_HOST>/auth/realms/<YOUR_REALM>/protocol/openid-connect/token" \
  -d "client_id=<YOUR_CLIENT_ID>" \
  -d "client_secret=<YOUR_CLIENT_SECRET>" \
  -d "grant_type=client_credentials"
```

成功すると、Keycloakは以下のようなJSONレスポンスを返します。

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "expires_in": 300,
  "refresh_expires_in": 0,
  "token_type": "Bearer",
  "not-before-policy": 0,
  "scope": "profile email"
}
```

取得した`access_token`を、APIリクエストの`Authorization`ヘッダーに含めてください。

```bash
curl -X GET \
  http://localhost:8080/api/v1/users \
  -H "Authorization: Bearer <YOUR_ACCESS_TOKEN>"
```

## ロールベースアクセス制御 (RBAC)

Fomage APIは、Keycloakのロールを利用してエンドポイントへのアクセス制御を行います。

- **ロールの定義**: Keycloakの管理コンソールで、`admin`や`viewer`といったロールを定義します。
- **ロールのマッピング**: クライアントやユーザーに適切なロールを割り当てます。
- **トークンの検証**: Fomage APIは、リクエストに含まれるアクセストークンを検証し、トークン内のロール情報に基づいてアクセスを許可または拒否します。

例えば、特定の管理用エンドポイントは`admin`ロールを持つクライアントからのリクエストのみを許可する、といった制御が可能です。 