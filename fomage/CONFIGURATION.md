# 環境変数設定

このドキュメントでは、`fomage`アプリケーションの設定に使用される環境変数について説明します。

## 概要

`fomage`は、動作をカスタマイズするために環境変数を使用します。これらの変数は、`.env`ファイルに定義するか、DockerやKubernetesなどの実行環境で直接設定することができます。

## 主要な環境変数

### データベース設定

| 変数名 | 説明 | デフォルト値 | 必須 |
| :--- | :--- | :--- | :--- |
| `MONGODB_URI` | 接続するMongoDBインスタンスのURI。ユーザー名、パスワード、ホスト、ポートなどを含みます。 | `mongodb://localhost:27017` | はい |
| `MONGODB_DATABASE` | `fomage`が使用するデータベース名。 | `fomage` | はい |

**設定例:**
```
MONGODB_URI=mongodb://admin:password123@mongodb-service:27017/my-fomage-db
MONGODB_DATABASE=my-fomage-db
```

### サーバー設定

| 変数名 | 説明 | デフォルト値 | 必須 |
| :--- | :--- | :--- | :--- |
| `SERVER_PORT` | アプリケーションがリッスンするポート番号。 | `8080` | いいえ |

**設定例:**
```
SERVER_PORT=8888
```

### ログ設定

| 変数名 | 説明 | デフォルト値 | 必須 |
| :--- | :--- | :--- | :--- |
| `LOG_LEVEL` | アプリケーションのログレベル。`DEBUG`, `INFO`, `WARN`, `ERROR`のいずれかを設定できます。 | `INFO` | いいえ |

**設定例:**
```
LOG_LEVEL=DEBUG
```

### 認証設定

| 変数名 | 説明 | デフォルト値 | 必須 |
| :--- | :--- | :--- | :--- |
| `API_KEY_SECRET` | APIキーを生成・検証するための秘密鍵。本番環境では必ずランダムで安全な文字列に変更してください。 | `default-secret-key` | はい |

**設定例:**
```
API_KEY_SECRET=a-very-long-and-secure-random-string
```

## 設定方法

### .envファイルを使用する

ローカルでの開発時には、プロジェクトのルートディレクトリに`.env`ファイルを作成して環境変数を設定するのが便利です。

```bash
# .env
MONGODB_URI=mongodb://localhost:27017
MONGODB_DATABASE=fomage_dev
LOG_LEVEL=DEBUG
API_KEY_SECRET=local-dev-secret
```

### Kubernetes

Kubernetesでは`ConfigMap`や`Secret`を使用して環境変数をコンテナに渡します。`fomage`では、設定に応じて`ConfigMap`と`Secret`を使い分けることを推奨します。

**ConfigMap (一般設定):**
```yaml
# fomage-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fomage-config
data:
  MONGODB_DATABASE: "fomage"
  SERVER_PORT: "8080"
  LOG_LEVEL: "INFO"
```

**Secret (機密情報):**
```yaml
# fomage-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: fomage-secret
type: Opaque
data:
  # base64エンコードした値を設定
  MONGODB_URI: "bW9uZ29kYjovL2FkbWluOnBhc3N3b3JkMTIzQG1vbmdvZGItc2VydmljZToyNzAxNy9mb21hZ2U="
  API_KEY_SECRET: "YS12ZXJ5LWxvbmctYW5kLXNlY3VyZS1yYW5kb20tc3RyaW5n"
``` 