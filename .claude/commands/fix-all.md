---
description: "全ての静的解析エラーを自動修正"
---

# /fix-all

**ultrathink**

プロジェクト全体の静的解析エラーを自動修正してください。
すべてのエラーが 0 件になるまで修正を繰り返します。

## 1. Python コードの修正

- 必ず `python-expert` エージェントを使用する
- ruff format、ruff check --fix を実行
- mypy のエラーを修正
- bandit の警告を修正
- すべてのエラーが 0 件になるまで繰り返す

## 2. Terraform コードの修正

- 必ず `terraform-expert` エージェントを使用する
- terraform fmt を実行
- tflint の警告を修正
- tfsec / checkov の警告を修正
- すべてのエラーが 0 件になるまで繰り返す

## 3. 最終確認

- 必ず `quality-reviewer` エージェントを使用して確認する
- 修正によって新たな問題が発生していないことを確認する
- テストがすべて通過することを確認する

## 対象

$ARGUMENTS

（指定がない場合はプロジェクト全体を対象とする）
