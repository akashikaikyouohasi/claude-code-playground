---
description: "ブランチ名を自動生成して作成"
---

# /create-branch

**ultrathink**

タスク内容に基づいて、適切なブランチ名を生成して作成してください。

## 手順

1. タスク内容を分析する
2. Conventional Branch に従ったブランチ名を生成する
3. ブランチを作成してチェックアウトする

## ブランチ命名規約

- `feature/<機能名>` - 新機能
- `fix/<バグ内容>` - バグ修正
- `refactor/<対象>` - リファクタリング
- `docs/<対象>` - ドキュメント
- `chore/<内容>` - 雑務

## 例

```
feature/user-authentication
fix/login-validation-error
refactor/api-client
```

## 対象

$ARGUMENTS
