# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Language Preference

**IMPORTANT: このプロジェクトでは、すべての説明、コメント、ドキュメントを日本語で記述してください。**

- ドキュメント: 日本語で記述
- Git commitメッセージ: 日本語で記述
- Claude Codeとのやり取り: 日本語で応答

## Project Overview

このリポジトリは、チームのドキュメント管理サイトです。MkDocsを使用して静的サイトを生成しています。

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
