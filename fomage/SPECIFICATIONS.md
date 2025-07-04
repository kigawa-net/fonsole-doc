# Fomage 管理パネル仕様書

## 1. 概要

このドキュメントは、`fomage`管理パネルの仕様を定義します。`fomage`は`fonsole`のバックアップデータを管理するためのWebベースのインターフェースです。

## 2. 目的

- `fonsole`が生成したMongoDBのバックアップデータを直感的に操作できるUIを提供する。
- データの閲覧、検索、削除などの基本的なデータ操作を可能にする。
- データベースの状態やプロジェクトの統計情報を可視化するダッシュボード機能を提供する。

## 3. 機能要件

### 3.1. 認証機能

- [ ] Keycloakを利用した認証
- [ ] OpenID Connect (OIDC) の認可コードフローを利用
- [ ] シングルサインオン (SSO) の実現
- [ ] ログアウト機能 (Keycloakとの連携)

詳細は [認証・認可(AUTHENTICATION.md)](AUTHENTICATION.md) を参照。

### 3.2. ダッシュボード

- [ ] プロジェクトの統計情報を表示
    - [ ] 総バックアップ数: 全プロジェクトのバックアップ総数を表示します。
    - [ ] データベースのストレージサイズ: 使用中のストレージ容量を可視化します。
    - [ ] 最新のバックアップ日時: 最後のバックアップがいつ実行されたかを示します。
- [ ] アクティビティログの表示: 最近の操作履歴（ログイン、データ削除など）を表示します。

### 3.3. データ管理

- [ ] `fonsole`プロジェクト一覧の表示
    - [ ] プロジェクト名での検索機能: インクリメンタルサーチで高速にプロジェクトを絞り込めます。
- [ ] プロジェクトごとのコレクション一覧表示: 選択したプロジェクトに属するコレクションを一覧で表示します。
- [ ] コレクション内のドキュメント一覧表示
    - [ ] ページネーション機能: 大量のドキュメントを効率的に閲覧できます。
    - [ ] JSON形式でのデータ表示: Syntax Highlightingを適用し、見やすく表示します。
    - [ ] ドキュメントのフィルタリング機能 (クエリによる検索): MongoDBのクエリ構文に似た形式でドキュメントを検索できます。
- [ ] ドキュメントの削除
    - [ ] ドキュメントの削除 (確認ダイアログ付き): 誤操作を防ぐための確認モーダルを表示します。

### 3.4. スキーマ管理

- [ ] MongoDBのコレクションスキーマを自動で取得・表示: `mongoose`のスキーマ定義を元に、フィールド名、型、制約などを表示します。
- [ ] スキーマのバージョン管理 (将来的な機能): スキーマの変更履歴を追跡し、バージョン間の差分を確認できる機能です。

## 4. 非機能要件

### 4.1. パフォーマンス

- APIレスポンスタイム: 95%のAPIリクエストが500ms以内に応答すること。
- ページ読み込み時間: 主要なページの読み込み時間が2秒以内に完了すること。

### 4.2. セキュリティ

- [認証・認可(AUTHENTICATION.md)](AUTHENTICATION.md) に記載されたセキュリティ要件を遵守する。
- XSS、CSRFなどの一般的なWeb脆弱性対策を講じること。

### 4.3. ユーザビリティ

- 直感的で分かりやすいUIデザインを採用する。
- レスポンシブデザインに対応し、主要なデバイス（デスクトップ、タブレット）で正しく表示されること。
