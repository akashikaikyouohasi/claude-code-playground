---
name: quality-check
description: "コード品質チェックを実行。Use when: コードレビュー、品質確認、リファクタリング前後"
---

# コード品質チェック Skill

## 概要

Python および Terraform コードの品質を総合的にチェックします。

## トリガー条件

以下の場合に自動的に適用されます：
- 「品質チェック」「レビュー」というキーワードが含まれる場合
- コードの実装が完了した後
- プルリクエスト作成前

## チェック項目

### Python コード

1. **静的解析**
   ```bash
   ruff check .
   mypy --strict .
   bandit -r .
   ```

2. **コード品質**
   - 型ヒントの完全性
   - docstring の存在
   - 命名規則の一貫性
   - 関数の複雑度（循環的複雑度 < 10）

3. **テスト**
   ```bash
   pytest --cov=src --cov-report=term-missing
   ```

### Terraform コード

1. **静的解析**
   ```bash
   terraform fmt -check -recursive
   terraform validate
   tflint
   tfsec .
   checkov -d .
   ```

2. **コード品質**
   - 変数に description があるか
   - 適切なタグ付け
   - モジュール化の適切さ

## 出力フォーマット

```markdown
## 品質チェック結果

### サマリー
- 総合スコア: [A/B/C/D/F]
- 重大な問題: X 件
- 警告: Y 件
- 情報: Z 件

### 詳細

#### 重大な問題
1. [問題の説明と修正案]

#### 警告
1. [警告の説明]

#### 推奨事項
1. [改善提案]
```

## ルール

- 進捗宣言（「〜を確認しています」等）は行わない
- 問題点は具体的なファイル名と行番号を含める
- 修正案は実行可能なコードで示す
- 未使用コードは削除を推奨
