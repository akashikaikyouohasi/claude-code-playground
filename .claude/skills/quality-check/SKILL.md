---
description: コード変更を検出した際に品質チェックを促す
globs:
  - "src/**/*.py"
  - "tests/**/*.py"
  - "terraform/**/*.tf"
---

# Quality Check Skill

コードを変更しました。以下の品質チェックを忘れずに実行してください。

## Python

```bash
# フォーマット
uvx ruff format .

# リント
uvx ruff check --fix .

# 型チェック
uvx mypy . --install-types --non-interactive

# テスト
uv run pytest -v --cov=src --cov-report=term-missing
```

## Terraform

```bash
# フォーマット
terraform fmt -recursive

# リント
tflint -f compact .

# 検証
terraform validate
```

## 確認事項

- warning, error が 0 件になるまで修正
- テストがすべて通過
- カバレッジが低下していない
