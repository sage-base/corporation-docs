# セットアップ

ローカル環境でドキュメントを編集・プレビューするための手順です。

## 前提条件

- [uv](https://docs.astral.sh/uv/) （Pythonパッケージマネージャー）
- Git

!!! tip "uvのインストール"
    ```bash
    # macOS / Linux
    curl -LsSf https://astral.sh/uv/install.sh | sh

    # Windows
    powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```

## インストール手順

### 1. リポジトリをクローン

```bash
git clone <repository-url>
cd corporation-docs
```

### 2. ローカルサーバーの起動

```bash
uv run mkdocs serve
```

`uv run` が自動的に依存関係をインストールして実行します。ブラウザで [http://127.0.0.1:8000](http://127.0.0.1:8000) を開きます。

!!! tip "ホットリロード"
    ファイルを保存すると、ブラウザが自動的にリロードされます。

## ディレクトリ構成

```
corporation-docs/
├── docs/                 # ドキュメントソース
│   ├── index.md         # ホームページ
│   └── getting-started/ # 入門ガイド
├── mkdocs.yml           # MkDocs設定ファイル
├── pyproject.toml       # プロジェクト設定・依存関係
└── .github/
    └── workflows/
        └── deploy.yml   # GitHub Actions設定
```

## よく使うコマンド

| コマンド | 説明 |
|---------|------|
| `uv run mkdocs serve` | ローカルサーバー起動 |
| `uv run mkdocs build` | 静的サイトをビルド |
| `uv run mkdocs serve -a 127.0.0.1:8080` | 別ポートで起動 |

## トラブルシューティング

### uvがインストールされていない

上記「前提条件」のインストールコマンドを実行してください。

### ポートが使用中の場合

```bash
uv run mkdocs serve -a 127.0.0.1:8080
```
