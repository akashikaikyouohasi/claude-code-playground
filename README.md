# Claude Code Playground

Python と Terraform を扱うプロジェクトのための Claude Code 開発環境です。

## 環境構成

```
.
├── CLAUDE.md                           # プロジェクト全体の設定・規約
├── README.md                           # このファイル
├── .claude/
│   ├── commands/                       # スラッシュコマンド
│   │   ├── implement-python.md         # /implement-python
│   │   ├── implement-terraform.md      # /implement-terraform
│   │   ├── review.md                   # /review
│   │   └── fix-all.md                  # /fix-all
│   ├── skills/                         # Agent Skills
│   │   ├── quality-check/
│   │   │   └── SKILL.md                # 品質チェック
│   │   └── security-review/
│   │       └── SKILL.md                # セキュリティレビュー
│   └── subagents/                      # サブエージェント
│       ├── python-expert.md            # Python 専門家
│       ├── terraform-expert.md         # Terraform 専門家
│       └── quality-reviewer.md         # 品質レビュアー
├── src/                                # Python ソースコード
├── tests/                              # テストコード
└── terraform/                          # Terraform コード
    ├── modules/                        # 再利用可能なモジュール
    └── environments/                   # 環境別設定
```

## スラッシュコマンド

| コマンド | 説明 |
|---------|------|
| `/implement-python <機能>` | Python 機能の実装からレビューまで |
| `/implement-terraform <リソース>` | Terraform モジュールの実装からレビューまで |
| `/review <対象>` | 品質・セキュリティレビューの実行 |
| `/fix-all` | 全ての静的解析エラーを自動修正 |

## サブエージェント

| エージェント | 役割 |
|-------------|------|
| `python-expert` | Python コードの実装・リファクタリング |
| `terraform-expert` | Terraform コードの実装・IaC 設計 |
| `quality-reviewer` | コード品質・セキュリティのレビュー |

## Agent Skills

| Skill | 説明 | トリガー |
|-------|------|---------|
| `quality-check` | コード品質の総合チェック | 「品質」「レビュー」 |
| `security-review` | セキュリティ脆弱性チェック | 「セキュリティ」「脆弱性」 |

## 開発ワークフロー

### Python 開発

```bash
# 依存関係インストール
uv sync

# フォーマット＆リント
ruff format . && ruff check --fix .

# 型チェック
mypy --strict .

# テスト
pytest -v
```

### Terraform 開発

```bash
# フォーマット
terraform fmt -recursive

# 検証
terraform validate && tflint

# セキュリティチェック
tfsec . && checkov -d .

# プラン
terraform plan
```

## 使用例

```
# Python 機能の実装
> /implement-python ユーザー認証機能

# Terraform モジュールの実装
> /implement-terraform VPC モジュール

# コードレビュー
> /review src/auth/

# エラー自動修正
> /fix-all
```
