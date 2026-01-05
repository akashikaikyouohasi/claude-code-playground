---
description: "Terraformモジュールを実装・検証・レビューまで完結"
---

# /implement-terraform

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
2. **フォーマット**: terraform fmt を実行
3. **検証**: terraform validate を実行
4. **リント**: tflint を実行し、エラーを0件にする
5. **セキュリティチェック**: tfsec, checkov を実行
6. **レビュー**: 要件充足・品質・セキュリティを確認する
7. **修正**: 問題があれば修正し、2に戻る

## 実行コマンド

```bash
# フォーマット
terraform fmt -recursive

# 検証
terraform validate

# リント
tflint -f compact .

# セキュリティ
tfsec .
uvx checkov -d .
```

## 対象

$ARGUMENTS
