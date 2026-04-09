# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Language Preference

**IMPORTANT: このプロジェクトでは、すべての説明、コメント、ドキュメントを日本語で記述してください。**

- ドキュメント: 日本語で記述
- Git commitメッセージ: 日本語で記述
- Claude Codeとのやり取り: 日本語で応答

## Project Overview

このリポジトリは、sage-base チームのドキュメント管理サイトです。

### 技術スタック

- **静的サイトジェネレーター**: [MkDocs](https://www.mkdocs.org/) + [Material for MkDocs](https://squidfund.github.io/mkdocs-material/) テーマ
- **ホスティング**: GitHub Pages
- **デプロイ**: GitHub Actions（`main` ブランチへのpush時に自動デプロイ）
- **パッケージ管理**: [uv](https://docs.astral.sh/uv/)
- **Python**: >= 3.10
- **依存関係**: `mkdocs-material >= 9.0.0`

### デプロイの仕組み

`main` ブランチにpushすると、`.github/workflows/deploy.yml` が `mkdocs gh-deploy --force` を実行し、ビルド成果物を `gh-pages` ブランチにpushしてGitHub Pagesへ公開します。手動トリガー（`workflow_dispatch`）にも対応しています。

## Related Repositories

### Sagebase

- **GitHub**: https://github.com/sage-base/sagebase
- **概要**: 政治活動追跡アプリケーション

ドキュメント作成時にSagebaseのソースコード、アーキテクチャ、実装詳細を参照する必要がある場合は、`gh` CLIコマンドを使用してください。

```bash
gh repo view sage-base/sagebase
gh api repos/sage-base/sagebase/contents/CLAUDE.md --jq '.content' | base64 -d
```

## Quick Start

```bash
# 依存関係のインストール
uv sync

# 開発サーバーの起動
uv run mkdocs serve

# サイトのビルド（本番前確認）
uv run mkdocs build --strict
```

## Directory Structure

```
corporation-docs/
├── docs/                          # ドキュメントソース
│   ├── index.md                   # トップページ
│   ├── political-domain/          # 政治ドメイン知識（国会・地方議会・首長選挙）
│   ├── development/               # 開発ドキュメント
│   │   ├── data-creation/         # マスターデータ作成手順（10種 + リレーション7種）
│   │   └── llm-processing/        # LLM処理パイプライン（5種）
│   ├── organization/              # 組織運営（定款・規程）
│   │   ├── articles-of-incorporation/  # 定款 + 各項目の設定理由
│   │   └── regulations/           # 規程
│   └── getting-started/           # セットアップ・執筆ガイド
├── mkdocs.yml                     # MkDocs設定（nav・テーマ・プラグイン）
├── pyproject.toml                 # Python依存関係
└── uv.lock                        # 依存関係ロック
```

## Writing Guidelines

- ドキュメントは `docs/` ディレクトリ以下にMarkdownファイルとして作成
- 新しいページを追加した場合は `mkdocs.yml` の `nav` セクションを更新
- コードブロックには適切な言語指定を付ける
- ビルド確認は `uv run mkdocs build --strict` で行う（warningもエラー扱い）

### Material for MkDocs で利用可能な機能

mkdocs.yml で有効化済みの拡張機能:

- **Admonitions**: `!!! note`, `!!! warning`, `!!! tip` 等の注意書きブロック
- **Details**: `??? note` で折りたたみ可能なブロック
- **Tabs**: `=== "Tab1"` でタブ切り替え
- **Mermaid**: ` ```mermaid ` でダイアグラム描画
- **Code copy**: コードブロックにコピーボタン自動表示
- **Tables**: 標準Markdownテーブル
- **TOC**: `permalink: true` で各見出しにパーマリンク
