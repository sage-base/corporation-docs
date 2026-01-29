# SKILL.md - MkDocs 操作スキル

このファイルは、Claude Codeがこのプロジェクトで MkDocs を操作する際のスキル・手順をまとめたものです。

## 基本コマンド

### 開発サーバーの起動

```bash
uv run mkdocs serve
```

- `http://127.0.0.1:8000` でプレビューが開く
- ファイル保存時にホットリロードされる
- ポート変更: `uv run mkdocs serve -a 127.0.0.1:8080`

### サイトのビルド

```bash
uv run mkdocs build
```

- `site/` ディレクトリにビルド成果物が生成される（`.gitignore` 対象）
- ビルドエラーの確認に使用する

### デプロイ（手動）

```bash
uv run mkdocs gh-deploy --force
```

- 通常は GitHub Actions が自動実行するため、手動実行は不要
- `gh-pages` ブランチにビルド成果物をpushする

## ページの追加手順

### 1. Markdownファイルを作成

`docs/` 配下の適切なディレクトリに `.md` ファイルを作成する。

```
docs/
├── index.md                    # トップページ
├── politics/                   # 政治ドメイン
├── organization/               # 組織運営
│   ├── articles-of-incorporation/  # 定款
│   └── regulations/            # 規程
└── getting-started/            # 入門ガイド
```

ファイル命名規則:

- 小文字のみ
- スペースの代わりにハイフン `-`
- 拡張子は `.md`
- ディレクトリのトップページは `index.md`

### 2. mkdocs.yml の nav セクションを更新

新しいページは必ず `mkdocs.yml` の `nav` に追加する。追加しないとナビゲーションに表示されない。

```yaml
nav:
  - Home: index.md
  - セクション名:
      - ページ名: section/page.md
```

ネストの構造:

```yaml
nav:
  - 親セクション:
      - 概要: parent/index.md
      - 子セクション:
          - ページ: parent/child/page.md
```

## Markdown拡張機能（このプロジェクトで有効なもの）

### アドモニション（注意書き）

```markdown
!!! note "タイトル"
    内容をインデント4スペースで記述

!!! warning "警告"
    警告の内容

!!! tip "ヒント"
    ヒントの内容

!!! info "情報"
    情報の内容
```

折りたたみ可能なアドモニション:

```markdown
??? note "クリックして展開"
    折りたたまれた内容

???+ note "デフォルトで開いた状態"
    最初から展開された内容
```

### コードブロック

```markdown
```python
def hello():
    print("Hello")
```
```

- コードブロックにはコピーボタンが自動表示される（`content.code.copy` 有効）
- 行番号のアンカーリンクが有効（`anchor_linenums` 有効）

### タブ

```markdown
=== "タブ1"
    タブ1の内容

=== "タブ2"
    タブ2の内容
```

### テーブル

```markdown
| 列1 | 列2 | 列3 |
|-----|-----|-----|
| A   | B   | C   |
```

### 目次パーマリンク

見出しに自動的にパーマリンクアイコンが付与される（`toc.permalink` 有効）。

## mkdocs.yml の設定構造

```yaml
site_name: サイト名
site_description: 説明

theme:
  name: material          # Material for MkDocs テーマ
  language: ja            # 日本語UI
  palette: [...]          # ライト/ダークモード切替
  features: [...]         # ナビゲーション等の機能

plugins:
  - search:               # 検索プラグイン
      lang: ja            # 日本語検索対応

markdown_extensions: [...] # Markdown拡張

nav: [...]                 # ナビゲーション構造
```

### 主な theme.features

| 機能 | 説明 |
|------|------|
| `navigation.tabs` | トップレベルのセクションをタブで表示 |
| `navigation.sections` | サイドバーのセクションを展開表示 |
| `navigation.expand` | サイドバーのすべてのセクションをデフォルトで展開 |
| `navigation.top` | 「トップへ戻る」ボタンを表示 |
| `search.suggest` | 検索サジェスト |
| `search.highlight` | 検索結果のハイライト |
| `content.code.copy` | コードブロックにコピーボタンを追加 |

## よくある操作パターン

### 新しいセクションを追加する

1. `docs/` 配下に新しいディレクトリを作成
2. `index.md` を作成してセクションの概要ページとする
3. `mkdocs.yml` の `nav` にセクションを追加

### 既存ページを編集する

1. `docs/` 配下の該当 `.md` ファイルを編集する
2. ナビゲーション上の位置を変えない限り `mkdocs.yml` の変更は不要

### ビルドエラーの確認

```bash
uv run mkdocs build --strict
```

`--strict` を付けると警告もエラーとして扱い、リンク切れなどを検出できる。
