---
description: "Python機能を実装・テスト・レビューまで完結"
---

# /implement-python

**ultrathink**

指定されたタスクに対して、CLAUDE.md に従って Plan から実装まで完結させてください。

## ワークフロー

### 1. Plan モード

まず Plan モードに入り、以下を実施：

- コードベースを調査し、影響範囲を把握
- 実装方針を策定
- ユーザーの承認を得る

### 2. 実装フェーズ

承認後、以下を実行：

1. **実装**: 要件に基づいてコードを実装する
2. **静的解析**: フォーマット・リント・型チェックを実行し、エラーを0件にする
3. **テスト**: テストを実行し、全て通過させる
4. **セキュリティチェック**: 脆弱性検査を実行する
5. **レビュー**: 要件充足・品質・セキュリティを確認する
6. **修正**: 問題があれば修正し、2に戻る

## 実行コマンド

```bash
# フォーマット
uvx ruff format .

# リント
uvx ruff check --fix .

# 型チェック
uvx mypy . --install-types --non-interactive

# テスト
uv run pytest -v --cov=src --cov-report=term-missing

# セキュリティ
uvx bandit -r . -lll
```

## 対象

$ARGUMENTS
