---
description: セキュリティに関わる実装を検出した際に自動で発動。脆弱性チェックを促す。
globs:
  - "**/*.py"
  - "**/*.tf"
---

# Security Check Skill

セキュリティに関わる実装を検出しました。以下を確認してください。

## Python

認証・認可、ユーザー入力、外部通信に関わるコードを変更した場合：

```bash
# セキュリティ検査
uvx bandit -r . -lll

# 依存脆弱性監査
uv export -o .tmp-requirements.txt
uvx pip-audit -r .tmp-requirements.txt --strict
rm .tmp-requirements.txt
```

### チェック項目

- [ ] SQL インジェクション対策（パラメータ化クエリ）
- [ ] XSS 対策（出力エスケープ）
- [ ] CSRF 対策（トークン検証）
- [ ] 入力値バリデーション
- [ ] 機密情報のハードコード禁止
- [ ] 適切な例外処理

## Terraform

インフラ設定を変更した場合：

```bash
# セキュリティ検査
tfsec .
uvx checkov -d .
```

### チェック項目

- [ ] 暗号化設定（S3, RDS, EBS）
- [ ] パブリックアクセス無効化
- [ ] セキュリティグループの最小権限
- [ ] IAM ポリシーの最小権限
- [ ] ログ・監査設定の有効化
- [ ] シークレットのハードコード禁止

## 必須

セキュリティに関わる変更では、上記のチェックを実行してからコミットしてください。
