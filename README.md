# Claude Code Playground

Python と Terraform を扱うプロジェクトのための Claude Code 開発環境です。

## 実装方針

このプロジェクトでは、**基本的にエージェントに実装を任せる**方針を採用しています。

- 開発者はスラッシュコマンドでタスクを指示するだけ
- 実装・テスト・静的解析・レビューはエージェントが自動で実行
- 問題があればエージェントが修正を繰り返す

## 初回セットアップ

### 必要なツール

```bash
# Python
uv          # パッケージマネージャ
ruff        # フォーマッター・リンター
mypy        # 型チェック
bandit      # セキュリティ検査

# Terraform
terraform   # IaC ツール
tflint      # リンター
tfsec       # セキュリティ検査
checkov     # セキュリティ検査
```

### インストール例（macOS）

```bash
# Python ツール
brew install uv
uv tool install ruff mypy bandit

# Terraform ツール
brew install terraform tflint tfsec
pip install checkov
```

### プロジェクト初期化

```bash
# Python 依存関係
uv sync

# Terraform 初期化（environments/dev など）
cd terraform/environments/dev
terraform init
```

## スラッシュコマンド

| コマンド | 説明 |
|---------|------|
| `/implement-python <機能>` | Python 機能の実装・テスト・レビューまで完結 |
| `/implement-terraform <リソース>` | Terraform モジュールの実装・検証・レビューまで完結 |

## サブエージェント

| エージェント | 役割 |
|-------------|------|
| `python-expert` | Python コードの実装・テスト・静的解析 |
| `terraform-expert` | Terraform コードの実装・セキュリティ検証 |
| `quality-reviewer` | コード品質・セキュリティのレビュー |

## 環境構成

```
.
├── CLAUDE.md                           # プロジェクト設定・規約
├── .claude/
│   ├── commands/                       # スラッシュコマンド
│   ├── settings.json                   # Hooks 設定（自動フォーマット）
│   └── subagents/                      # サブエージェント定義
├── src/                                # Python ソースコード
├── tests/                              # テストコード
└── terraform/                          # Terraform コード
    ├── modules/                        # 再利用可能なモジュール
    └── environments/                   # 環境別設定
```

## 参考リンク

- [Claude Code の Agent Skills と Subagent の違いを理解する](https://zenn.dev/mkj/articles/868e0723efa060)
- [Claude Code の Agent Skills についての理解と実践](https://syu-m-5151.hatenablog.com/entry/2025/12/19/173309)
