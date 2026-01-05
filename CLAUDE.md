# CLAUDE.md - プロジェクト設定

## プロジェクト概要

このリポジトリは Python と Terraform を扱うプロジェクトです。

## 実装ワークフロー

**Plan → 実装 → レビュー → 修正 → 再レビュー** のループを必ず実行する。

### 0. Plan モード（必須）

実装を開始する前に、必ず Plan モードで設計を行う。

- コードベースを調査し、影響範囲を把握する
- 実装方針を策定し、ユーザーの承認を得る
- 承認後に実装フェーズに移行する

Plan モードをスキップして良いケース：
- 単純なタイポ修正
- 1ファイル内の軽微な修正
- ユーザーが明示的に「すぐ実装して」と指示した場合

### 1. 実装フェーズ（フィードバックループ）

実装後は必ず検証を行い、問題があれば修正を繰り返す。このフィードバックループが品質を担保する。

1. **実装**: 要件に基づいてコードを実装する
2. **検証**: 以下をすべて実行し、結果を確認する
   - フォーマット・リント・型チェック（エラー 0 件にする）
   - テスト実行（全て通過させる）
   - セキュリティ検査
3. **修正**: 問題があれば修正し、2 に戻る

**すべてのチェックをクリアするまで繰り返す。検証なしで完了としない。**

## 技術スタック

### Python
- **バージョン**: Python 3.11+
- **パッケージマネージャ**: `uv` を使用（pip, poetry は使用しない）
- **フォーマッター**: `ruff format`
- **リンター**: `ruff check`
- **型チェック**: `mypy --strict`
- **セキュリティ**: `bandit`
- **テストフレームワーク**: `pytest`

### Terraform
- **バージョン**: Terraform 1.5+
- **フォーマッター**: `terraform fmt`
- **検証**: `terraform validate`
- **セキュリティ**: `tfsec`, `checkov`
- **リンター**: `tflint`

## 実行コマンド

### Python

```bash
# フォーマット
uvx ruff format .

# リント（自動修正あり）
uvx ruff check --fix .

# 型チェック
uvx mypy . --install-types --non-interactive

# セキュリティ検査
uvx bandit -r . -lll

# 依存脆弱性監査
uv export -o .tmp-requirements.txt
uvx pip-audit -r .tmp-requirements.txt --strict
rm .tmp-requirements.txt

# テスト
uv run pytest -v --cov=src --cov-report=term-missing
```

### Terraform

```bash
# フォーマット
terraform fmt -recursive

# 検証
terraform validate

# リント
tflint -f compact .

# セキュリティチェック
tfsec .
uvx checkov -d .

# プラン
terraform plan -out=tfplan
```

## コーディング規約

### 共通
- 関数は集中して小さく保つ（20行を目安）
- 一つの関数は一つの責務を持つ
- 既存のパターンを正確に踏襲する
- 未使用のコード・インポート・変数は即座に削除
- 後方互換性の名目で使用しなくなったコードを残さない

### Python
- PEP8 準拠
- 全ての関数・メソッドに型ヒントを必須とする
- `typing` モジュールは使用せず、PEP 585 の組み込みジェネリクスを使用する
- 変数名・関数名は `snake_case`
- クラス名は `PascalCase`
- 定数は `UPPER_SNAKE_CASE`
- docstring は Google スタイル、日本語で記載
- 1行の最大文字数: 88文字（ruff デフォルト）

### Terraform
- リソース名は `snake_case`
- 変数には `description` を必須とする
- モジュールは `modules/` ディレクトリに配置
- 環境ごとの設定は `environments/` ディレクトリに配置
- `locals` ブロックでタグを一元管理
- 必須タグ: `Environment`, `Project`, `ManagedBy`

### 型ヒント（Python）

```python
# Good: PEP 585 組み込みジェネリクス
def process_items(items: list[str]) -> dict[str, int]:
    ...

# Bad: typing モジュール
from typing import List, Dict
def process_items(items: List[str]) -> Dict[str, int]:
    ...
```

## パッケージ管理（Python）

- `uv` のみを使用し、`pip` は絶対に使わない
- インストール: `uv add package`
- 開発依存: `uv add --dev package`
- ツール実行: `uv run tool` または `uvx tool`
- 禁止: `uv pip install`、`@latest` 構文
- ライセンス: 非コピーレフト（Apache, MIT, BSD）を優先。それ以外は確認を取る

## セキュリティルール

### Python
- SQL インジェクション、XSS、CSRF 対策を徹底
- 入力値は必ずバリデーション
- 機密情報は環境変数または Secrets Manager で管理
- `except Exception` の広範なキャッチは禁止

### Terraform
- ハードコードされたシークレットは禁止
- S3 バケットはデフォルトで暗号化・パブリックアクセスブロック
- RDS はデフォルトで暗号化・パブリックアクセス無効
- セキュリティグループは最小権限（0.0.0.0/0 は原則禁止）
- IAM ポリシーは最小権限（ワイルドカードは原則禁止）

## 禁止事項

### Python
- `print` デバッグを本番コードに残すこと
- `# type: ignore` の濫用
- グローバル変数の使用
- 循環インポート
- マジックナンバー（定数として定義する）

### Terraform
- `terraform apply -auto-approve` を本番環境で使用
- ステートファイルをローカルに保存（本番環境）
- ハードコードされた AMI ID や IP アドレス
- `count` より `for_each` を優先（順序依存を避ける）

## コメント・ドキュメント方針

- 進捗・完了の宣言を書かない（例：「XX を実装」は禁止）
- 日付や相対時制を書かない（例：「2025-09-28 に実装」は禁止）
- 「何をしたか」ではなく「目的・仕様・入出力・挙動・制約」を記述する
- コメントや docstring は日本語で記載する

## Git ルール

- コミットメッセージは日本語で記述
- プレフィックス: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- 機能追加時は必ずテストも追加
- secrets や credentials は絶対にコミットしない

## ディレクトリ構成

```
.
├── CLAUDE.md                 # このファイル
├── README.md                 # プロジェクト説明
├── .claude/
│   ├── commands/             # スラッシュコマンド
│   ├── skills/               # Skills（必須チェック）
│   ├── subagents/            # サブエージェント（バックグラウンドタスク）
│   └── settings.json         # Hooks 設定
├── src/                      # Python ソースコード
├── tests/                    # テストコード
├── terraform/
│   ├── modules/              # 再利用可能なモジュール
│   └── environments/         # 環境別設定
└── pyproject.toml            # Python プロジェクト設定
```

## Claude Code 運用方針

### 基本方針

- **メインエージェント**が CLAUDE.md のルールに従って実装を行う
- **サブエージェント**はバックグラウンドで独立実行するタスクに使用
- **Skills**は忘れてはならない必須チェック（セキュリティなど）に使用

### サブエージェントの用途

- アプリケーションの検証（テスト実行）
- コードのシンプル化・リファクタリング
- ビルド最適化
- オンコール・障害調査

### 継続的改善

- Claude が間違った動作をしたら、即座に CLAUDE.md に追記する
- 同じ作業を3回以上繰り返したら、スラッシュコマンド化を提案する
