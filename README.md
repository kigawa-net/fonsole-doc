# fonsole-doc ドキュメント

## 目次
1. 構成概要
2. 各ディレクトリ・ファイルの役割
3. ドキュメント記述ルール
4. 今後の改善案
5. 参考リンク

---

## 1. 構成概要

このリポジトリは、`fonsole`（バックアップCLI）、`fomage`（Web UI/管理API）、および共通データモデルのドキュメントで構成されています。

```
fonsole-doc/
├── shared/    # 共通データモデル
├── fomage/    # Web UI・API管理ツール
├── fonsole/   # バックアップCLIツール
└── README.md  # 本ドキュメント
```

---

## 2. 各ディレクトリ・ファイルの役割

### shared/
- data-models.md  
  fomage・fonsoleで共通利用するデータモデル定義
- mongodb-models.md  
  fonsoleで利用されるMongoDBコレクションのデータモデル定義

### fomage/
- README.md  
  fomage全体の概要・構成・利用方法・開発ガイド
- quickstart.md  
  fomageのセットアップ手順
- ARCHITECTURE.md  
  システム設計・技術スタック
- CONFIGURATION.md  
  環境変数・設定方法
- DATA_MODELS.md  
  APIのデータモデル定義
- DEVELOPMENT.md  
  開発環境構築・コーディング規約
- ENDPOINTS.md  
  APIエンドポイント一覧
- KUBERNETES.md  
  デプロイ・運用手順
- API_OVERVIEW.md  
  API概要・認証・共通仕様

### fonsole/
- README.md  
  fonsole全体の概要・使い方・設定方法
- quickstart.md  
  fonsoleの基本的な使い方・セットアップ
- api.md  
  fonsoleが利用するMongoDBコレクション・スキーマ説明

---

## 3. ドキュメント記述ルール（統一案）

- すべてのドキュメントはMarkdown形式で記述
- 章立て・目次を必ず記載
- コード例・JSON例はコードブロックで明示
- 用語や略語は初出時に説明
- ファイル間リンクは相対パスで統一

---

## 4. 今後の改善案

### ファイル名・ディレクトリ構成の見直し案
- `shared/` など、より直感的な名称に統一
- `data-models.md`/`mongodb-models.md` など小文字・ハイフン区切りに統一
- `fomage/`・`fonsole/`配下のドキュメントも命名規則を統一

### 不要ファイルの削除提案
- ルートの空のREADME.mdは本ドキュメントで上書き
- 使われていない/重複しているドキュメントがあれば削除

---

## 5. 参考リンク

- [fomage/README.md](fomage/README.md)
- [fonsole/README.md](fonsole/README.md)
- [shared/data-models.md](shared/data-models.md)
- [shared/mongodb-models.md](shared/mongodb-models.md)

---

本ドキュメントは随時更新されます。ご意見・ご要望はIssueまたはPull Requestでお寄せください。
