# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Language Preference

**IMPORTANT: このプロジェクトでは、すべての説明、コメント、ドキュメントを日本語で記述してください。**

- ドキュメント: 日本語で記述
- Git commitメッセージ: 日本語で記述
- Claude Codeとのやり取り: 日本語で応答

## Project Overview

このリポジトリは、チームのドキュメント管理サイトです。

### 技術スタック

- **静的サイトジェネレーター**: [MkDocs](https://www.mkdocs.org/) + [Material for MkDocs](https://squidfund.github.io/mkdocs-material/) テーマ
- **ホスティング**: GitHub Pages
- **デプロイ**: GitHub Actions（`main` ブランチへのpush時に自動デプロイ）
- **パッケージ管理**: [uv](https://docs.astral.sh/uv/)

### デプロイの仕組み

`main` ブランチにpushすると、`.github/workflows/deploy.yml` が `mkdocs gh-deploy --force` を実行し、ビルド成果物を `gh-pages` ブランチにpushしてGitHub Pagesへ公開します。手動トリガー（`workflow_dispatch`）にも対応しています。

## Related Repositories

このドキュメントサイトは、以下のアプリケーションに関するドキュメントを管理しています。

### Sagebase

- **GitHub**: https://github.com/sage-base/sagebase
- **概要**: 政治活動追跡アプリケーション（Political Activity Tracking Application）

ドキュメント作成時にSagebaseのソースコード、アーキテクチャ、実装詳細を参照する必要がある場合は、`gh` CLIコマンドを使用してリポジトリの情報を取得してください。

```bash
# リポジトリの内容を確認
gh repo view sage-base/sagebase

# ファイルの内容を取得
gh api repos/sage-base/sagebase/contents/CLAUDE.md --jq '.content' | base64 -d

# ディレクトリ構造を確認
gh api repos/sage-base/sagebase/contents/src
```

## Quick Start

```bash
# 開発サーバーの起動
uv run mkdocs serve

# サイトのビルド
uv run mkdocs build

# 依存関係のインストール
uv sync
```

## Directory Structure

```
corporation-docs/
├── docs/           # ドキュメントのソースファイル（Markdown）
├── mkdocs.yml      # MkDocs設定ファイル
├── pyproject.toml  # Python依存関係
└── uv.lock         # 依存関係のロックファイル
```

## Writing Guidelines

- ドキュメントは `docs/` ディレクトリ以下にMarkdownファイルとして作成
- 新しいページを追加した場合は `mkdocs.yml` の `nav` セクションを更新
- コードブロックには適切な言語指定を付ける
