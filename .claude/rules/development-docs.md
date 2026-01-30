---
paths:
  - "docs/development/**"
---

# 開発ドキュメント作成ルール

`docs/development/` 配下のドキュメントは、Sagebase（https://github.com/sage-base/sagebase）の開発に関する内容を扱う。
ドキュメント作成・編集時は、以下の手順でSagebaseリポジトリから最新の情報を取得して内容に反映すること。

## ソースコード参照手順

```bash
# リポジトリの概要を確認
gh repo view sage-base/sagebase

# CLAUDE.mdからプロジェクトの規約・構成を取得
gh api repos/sage-base/sagebase/contents/CLAUDE.md --jq '.content' | base64 -d

# ディレクトリ構造を確認
gh api repos/sage-base/sagebase/contents/{パス}

# 特定ファイルの内容を取得
gh api repos/sage-base/sagebase/contents/{ファイルパス} --jq '.content' | base64 -d

# 最新のコミット履歴を確認
gh api repos/sage-base/sagebase/commits --jq '.[0:10] | .[] | "\(.sha[0:7]) \(.commit.message | split("\n")[0])"'
```

## 記述方針

- 実装の説明を書く際は、必ずSagebaseの該当ソースコードを確認してから記述する
- 推測で書かず、実際のコード・設定ファイルに基づいた正確な内容を記載する
- アーキテクチャ図やデータモデルはSagebaseの実装と整合性を保つ
