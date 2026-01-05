# Claude Code Playground

Python と Terraform を扱うプロジェクトのための Claude Code 開発環境です。

## 実装方針

このプロジェクトでは、**メインエージェントが CLAUDE.md に従って実装を行う**方針を採用しています。

- 開発者はスラッシュコマンドでタスクを指示するだけ
- メインエージェントが CLAUDE.md のルールに従って実装・テスト・レビューを実行
- サブエージェントはバックグラウンドでの検証・分析に使用
- Skills が必須チェック（セキュリティなど）を自動で促す

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

### 実装系

| コマンド | 説明 |
|---------|------|
| `/implement-python <機能>` | Python 機能の実装・テスト・レビューまで完結 |
| `/implement-terraform <リソース>` | Terraform モジュールの実装・検証・レビューまで完結 |

### Git 操作系

| コマンド | 説明 |
|---------|------|
| `/create-branch <タスク内容>` | ブランチ名を自動生成して作成 |
| `/commit` | コミットメッセージを自動生成してコミット |
| `/create-pr` | Pull Request を自動作成 |

### ユーティリティ

| コマンド | 説明 |
|---------|------|
| `/explain <対象>` | 実装内容を詳細に説明 |
| `/investigate <障害>` | 障害・エラーの原因を調査 |
| `/sync-docs <対象>` | コードとドキュメントを同期 |

## サブエージェント（バックグラウンドタスク用）

| エージェント | 役割 |
|-------------|------|
| `app-validator` | テスト・静的解析をバックグラウンドで実行 |
| `code-simplifier` | コードの複雑度分析・シンプル化提案 |
| `build-optimizer` | CI/CD・Docker・依存関係の最適化提案 |
| `incident-investigator` | 障害調査・根本原因の特定 |

## Skills（自動チェック）

| Skill | 役割 |
|-------|------|
| `security-check` | セキュリティに関わる変更で脆弱性チェックを促す |
| `quality-check` | コード変更時に品質チェックを促す |

## 環境構成

```
.
├── CLAUDE.md                           # プロジェクト設定・規約（メインエージェントが参照）
├── .claude/
│   ├── commands/                       # スラッシュコマンド
│   ├── skills/                         # Skills（必須チェック）
│   ├── subagents/                      # サブエージェント（バックグラウンドタスク）
│   └── settings.json                   # Hooks 設定（自動フォーマット）
├── src/                                # Python ソースコード
├── tests/                              # テストコード
└── terraform/                          # Terraform コード
    ├── modules/                        # 再利用可能なモジュール
    └── environments/                   # 環境別設定
```

## 参考リンク

- [Claude Code の Agent Skills と Subagent の違いを理解する](https://zenn.dev/mkj/articles/868e0723efa060)
- [Claude Code の Agent Skills についての理解と実践](https://syu-m-5151.hatenablog.com/entry/2025/12/19/173309)
