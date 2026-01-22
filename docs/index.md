# Team Documentation

チームのドキュメント管理サイトへようこそ！

## このサイトについて

このサイトは [MkDocs](https://www.mkdocs.org/) と [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) を使用して構築されています。

## クイックスタート

### ローカルでプレビューする

```bash
# ローカルサーバーの起動（uvが依存関係を自動解決）
uv run mkdocs serve
```

ブラウザで `http://127.0.0.1:8000` を開くと、リアルタイムプレビューが確認できます。

### ドキュメントを編集する

1. `docs/` ディレクトリ内の Markdown ファイルを編集
2. 変更をコミット & プッシュ
3. GitHub Actions が自動でサイトをビルド & デプロイ

## ドキュメント構成

| ディレクトリ | 内容 |
|------------|------|
| `docs/` | ドキュメントのソースファイル |
| `docs/getting-started/` | 入門ガイド |

## 貢献方法

1. ブランチを作成
2. ドキュメントを追加・編集
3. プルリクエストを作成
4. レビュー後にマージ
