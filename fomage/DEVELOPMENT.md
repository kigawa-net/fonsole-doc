# 開発ガイド

このドキュメントは、Fomageプロジェクトの開発者向けの詳細なガイドです。

## 目次

- [開発環境のセットアップ](#開発環境のセットアップ)
- [プロジェクト構造](#プロジェクト構造)
- [コーディング規約](#コーディング規約)
- [テスト](#テスト)
- [デバッグ](#デバッグ)
- [リリースプロセス](#リリースプロセス)
- [トラブルシューティング](#トラブルシューティング)

## 開発環境のセットアップ

### 必要なツール

- **Java**: OpenJDK 17以上
- **Gradle**: 8.0以上
- **IDE**: IntelliJ IDEA（推奨）または VS Code
- **Git**: 最新版
- **MongoDB**: 5.0以上（ローカル開発用）

### IDE設定

#### IntelliJ IDEA

1. **Kotlinプラグインの有効化**
   - Settings > Plugins > Kotlin を有効にする

2. **Gradle設定**
   - Settings > Build Tools > Gradle
   - Gradle JVM: 17以上を選択
   - Build and run using: Gradle
   - Run tests using: Gradle

3. **コードスタイル設定**
   - Settings > Editor > Code Style > Kotlin
   - プロジェクトの`.editorconfig`ファイルを使用

#### VS Code

1. **拡張機能のインストール**
   - Kotlin Language
   - Gradle for Java
   - MongoDB for VS Code

2. **設定**
   - `settings.json`でKotlin関連の設定を追加

### 環境変数の設定

開発用の環境変数を設定します：

```bash
# .env.local ファイルを作成
cp .env.example .env.local

# 以下の内容を編集
MONGODB_URI=mongodb://localhost:27017
MONGODB_DATABASE=fomage_dev
LOG_LEVEL=DEBUG
```

### ローカルでの起動手順

1.  **リポジトリをクローン**:
    ```bash
    git clone <repository-url>
    cd fomage
    ```

2.  **環境変数を設定**:
    `.env.example` をコピーして `.env` ファイルを作成し、環境に合わせて編集します。
    ```bash
    cp env.example .env
    ```

3.  **MongoDBを起動**:
    開発用にローカルでMongoDBを起動します。Dockerを使用すると簡単です。
    ```bash
    docker run -d \
      --name mongodb \
      -p 27017:27017 \
      -e MONGO_INITDB_ROOT_USERNAME=admin \
      -e MONGO_INITDB_ROOT_PASSWORD=password123 \
      mongo:7.0
    ```

4.  **アプリケーションをビルド**:
    ```bash
    ./gradlew build
    ```

5.  **アプリケーションを実行**:
    ```bash
    ./gradlew run
    ```
    ホットリロードを有効にして開発する場合は、`--continuous`フラグを追加します。
    ```bash
    ./gradlew run --continuous
    ```

## プロジェクト構造

```
fomage/
├── build.gradle.kts              # メインのビルド設定
├── buildSrc/                     # カスタムGradleプラグイン
│   ├── build.gradle.kts
│   ├── settings.gradle.kts
│   └── src/main/kotlin/
│       ├── fomage.app.gradle.kts
│       ├── fomage.common.gradle.kts
│       └── fomage.root.gradle.kts
├── gradle/
│   ├── libs.versions.toml        # 依存関係のバージョン管理
│   └── wrapper/
├── src/
│   ├── main/
│   │   └── kotlin/
│   │       └── net/kigawa/fomage/
│   │           └── Main.kt       # メインエントリーポイント
│   └── test/
│       └── kotlin/               # テストコード
├── docs/                         # ドキュメント
├── .gitignore
├── gradle.properties
├── gradlew
├── gradlew.bat
├── LICENSE
└── README.md
```

## コーディング規約

### Kotlin規約

- **命名規則**: キャメルケースを使用
  - クラス名: `PascalCase`
  - 関数・変数名: `camelCase`
  - 定数: `UPPER_SNAKE_CASE`

- **インデント**: 4スペース
- **行の長さ**: 最大120文字
- **インポート**: 未使用のインポートは削除

### コード例

```kotlin
package net.kigawa.fomage

import org.slf4j.LoggerFactory

class UserService(
    private val userRepository: UserRepository
) {
    private val logger = LoggerFactory.getLogger(UserService::class.java)
    
    suspend fun createUser(user: User): Result<User> {
        return try {
            val createdUser = userRepository.save(user)
            logger.info("User created: ${createdUser.id}")
            Result.success(createdUser)
        } catch (e: Exception) {
            logger.error("Failed to create user", e)
            Result.failure(e)
        }
    }
}
```

### コメント規約

- **KDoc**: 公開APIにはKDocコメントを記述
- **TODO**: 未実装部分にはTODOコメントを追加
- **FIXME**: 修正が必要な部分にはFIXMEコメントを追加

```kotlin
/**
 * ユーザーを作成します。
 *
 * @param user 作成するユーザー情報
 * @return 作成されたユーザー、またはエラー
 */
suspend fun createUser(user: User): Result<User>
```

## テスト

### アプリケーションの動作確認 (ヘルスチェック)

アプリケーションが起動したら、以下のコマンドで動作確認ができます。

```bash
# ヘルスチェック
curl http://localhost:8080/health

# アプリケーション情報
curl http://localhost:8080/api/v1/info
```

### テストの実行

```bash
# 全テストを実行
./gradlew test

# 特定のテストクラスのみ実行
./gradlew test --tests UserServiceTest

# テストレポートを生成
./gradlew test jacocoTestReport
```

### テスト規約

- **テストクラス名**: `{ClassName}Test`
- **テストメソッド名**: `should_{期待する動作}_when_{条件}`
- **テストファイル**: `src/test/kotlin/`に配置

### テスト例

```kotlin
package net.kigawa.fomage

import kotlinx.coroutines.test.runTest
import org.junit.jupiter.api.Test
import org.junit.jupiter.api.assertThrows
import kotlin.test.assertEquals

class UserServiceTest {
    
    @Test
    fun `should create user successfully when valid user data provided`() = runTest {
        // Given
        val userService = UserService(mockUserRepository)
        val user = User(name = "Test User", email = "test@example.com")
        
        // When
        val result = userService.createUser(user)
        
        // Then
        assert(result.isSuccess)
        assertEquals("Test User", result.getOrNull()?.name)
    }
    
    @Test
    fun `should throw exception when invalid email provided`() = runTest {
        // Given
        val userService = UserService(mockUserRepository)
        val user = User(name = "Test User", email = "invalid-email")
        
        // When & Then
        assertThrows<IllegalArgumentException> {
            userService.createUser(user)
        }
    }
}
```

### テストカバレッジ

- **目標**: 80%以上
- **レポート**: `build/reports/jacoco/test/html/index.html`

## デバッグ

### ログ出力

```kotlin
import org.slf4j.LoggerFactory

class MyClass {
    private val logger = LoggerFactory.getLogger(MyClass::class.java)
    
    fun someMethod() {
        logger.debug("Debug message")
        logger.info("Info message")
        logger.warn("Warning message")
        logger.error("Error message", exception)
    }
}
```

### デバッグ実行

```bash
# デバッグモードで実行
./gradlew run --debug-jvm

# リモートデバッグ
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar app.jar
```

### プロファイリング

```bash
# JFR（Java Flight Recorder）を使用
java -XX:+FlightRecorder -XX:StartFlightRecording=duration=60s,filename=profile.jfr -jar build/libs/fomage-all.jar
```

## リリースプロセス

### バージョン管理

- **セマンティックバージョニング**: `MAJOR.MINOR.PATCH`
- **開発版**: `MAJOR.MINOR.PATCH-SNAPSHOT`

### リリース手順

1. **バージョン更新**
   ```bash
   # build.gradle.ktsでバージョンを更新
   version = "1.0.0"
   ```

2. **テスト実行**
   ```bash
   ./gradlew clean test
   ```

3. **ビルド**
   ```bash
   ./gradlew clean build
   ```

4. **タグ作成**
   ```bash
   git tag -a v1.0.0 -m "Release version 1.0.0"
   git push origin v1.0.0
   ```

5. **Dockerイメージ作成**
   ```bash
   docker build -t kigawa01/fomage:1.0.0 .
   docker push kigawa01/fomage:1.0.0
   ```

## トラブルシューティング

### よくある問題

#### Gradleビルドエラー

```bash
# Gradleキャッシュをクリア
./gradlew clean
rm -rf ~/.gradle/caches/

# 依存関係を再ダウンロード
./gradlew --refresh-dependencies build
```

#### MongoDB接続エラー

1. MongoDBサービスが起動しているか確認
2. 接続文字列が正しいか確認
3. ファイアウォール設定を確認

#### メモリ不足エラー

```bash
# JVMヒープサイズを増加
./gradlew run -Dorg.gradle.jvmargs="-Xmx4g"
```

### ログの確認

```bash
# アプリケーションログ
tail -f logs/application.log

# Gradleビルドログ
./gradlew build --info
```

### パフォーマンス問題

1. **JVMオプションの調整**
   ```bash
   java -Xms512m -Xmx2g -XX:+UseG1GC -jar app.jar
   ```

2. **プロファイリングツールの使用**
   - JProfiler
   - VisualVM
   - Java Flight Recorder

## 貢献ガイドライン

### プルリクエスト

1. **ブランチ命名**: `feature/機能名` または `fix/修正内容`
2. **コミットメッセージ**: 明確で簡潔な説明
3. **テスト**: 新機能にはテストを追加
4. **ドキュメント**: 必要に応じてドキュメントを更新

### コードレビュー

- **必須**: 最低1人のレビューアーの承認
- **チェック項目**:
  - コードの品質
  - テストカバレッジ
  - ドキュメントの更新
  - セキュリティの考慮

## 参考資料

- [Kotlin公式ドキュメント](https://kotlinlang.org/docs/home.html)
- [Gradle公式ドキュメント](https://docs.gradle.org/)
- [MongoDB Kotlin Driver](https://mongodb.github.io/mongo-kotlin-driver/)
- [Kotlin Coroutines](https://kotlinlang.org/docs/coroutines-overview.html)

## モジュールの追加

新しいモジュールをプロジェクトに追加する手順は以下の通りです。

1.  `settings.gradle`にモジュールを追加します。
    ```gradle
    include 'new-module'
    ```

2.  新しいモジュールのための`build.gradle`を作成します。

3.  必要な依存関係を`build.gradle`に設定します。

## テストの実行

プロジェクトのテストはGradleを使用して実行します。

### 全モジュールのテスト

```bash
# 全モジュールのテストを一度に実行
./gradlew test
```

### 特定のモジュールのテスト

```bash
# Web UIモジュールのテスト
./gradlew :fomage:test

# APIモジュールのテスト
./gradlew :fomage-api:test
```

## 開発ワークフロー

一般的な開発フローは以下の通りです。

1.  **ブランチの作成**:
    `main`ブランチから新しい機能ブランチを作成します。
    ```bash
    git checkout -b feature/your-new-feature
    ```

2.  **コードの編集**:
    IDEでコードを編集し、機能を追加・修正します。

3.  **テストの実行**:
    変更によって既存の機能が壊れていないか、テストを実行して確認します。
    ```bash
    ./gradlew test
    ```

4.  **コミット**:
    変更内容をコミットします。
    ```bash
    git add .
    git commit -m "feat: Add your new feature"
    ```

5.  **プルリクエストの作成**:
    リモートリポジトリにプッシュし、プルリクエストを作成してレビューを依頼します。
    ```bash
    git push origin feature/your-new-feature
    ```