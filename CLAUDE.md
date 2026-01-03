# CLAUDE.md - プロジェクト設定

## プロジェクト概要

このリポジトリは Python と Terraform を扱うプロジェクトです。

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

## コーディング規約

### Python
- PEP8 準拠
- 全ての関数・メソッドに型ヒントを必須とする
- 変数名・関数名は `snake_case`
- クラス名は `PascalCase`
- 定数は `UPPER_SNAKE_CASE`
- docstring は Google スタイル
- 1行の最大文字数: 88文字（ruff デフォルト）

### Terraform
- リソース名は `snake_case`
- 変数には `description` を必須とする
- モジュールは `modules/` ディレクトリに配置
- 環境ごとの設定は `environments/` ディレクトリに配置
- `locals` ブロックでタグを一元管理

## ディレクトリ構成

```
.
├── CLAUDE.md                 # このファイル
├── README.md                 # プロジェクト説明
├── .claude/                  # Claude Code 設定
│   ├── commands/             # スラッシュコマンド
│   ├── skills/               # Agent Skills
│   └── subagents/            # サブエージェント定義
├── src/                      # Python ソースコード
│   └── ...
├── tests/                    # テストコード
│   └── ...
├── terraform/                # Terraform コード
│   ├── modules/              # 再利用可能なモジュール
│   └── environments/         # 環境別設定
│       ├── dev/
│       ├── staging/
│       └── prod/
└── pyproject.toml            # Python プロジェクト設定
```

## 開発ワークフロー

### Python 開発
1. `uv sync` で依存関係をインストール
2. `ruff check --fix .` でリント＆自動修正
3. `ruff format .` でフォーマット
4. `mypy .` で型チェック
5. `pytest` でテスト実行

### Terraform 開発
1. `terraform fmt -recursive` でフォーマット
2. `terraform validate` で構文検証
3. `tflint` でリント
4. `tfsec .` でセキュリティチェック
5. `terraform plan` で変更確認

## Git ルール

- コミットメッセージは英語で記述
- プレフィックス: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- 機能追加時は必ずテストも追加
- ドキュメント更新は機能追加と同時に行う

## 注意事項

- 未使用のコード・インポートは即座に削除
- TODO コメントは残さない（即座に対応するか Issue を作成）
- secrets や credentials は絶対にコミットしない
- `.env` ファイルは `.gitignore` に含める

## Claude Code 設計方針

### スラッシュコマンド

- **最小限のルーティングのみ**を行う
- 詳細な処理ロジックはサブエージェントに委譲する
- コマンドには具体的なコード例やコマンド例を書かない

### サブエージェント

- **詳細な処理ロジック**を担当する
- コーディング規約、実行コマンド、検証手順などを含める
- 専門分野ごとに分離する（python-expert, terraform-expert, quality-reviewer）

### 責務分離

```
スラッシュコマンド → ルーティング（何をどの順で呼ぶか）
サブエージェント   → 実行（どう実装・検証するか）
```
