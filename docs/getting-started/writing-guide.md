# 執筆ガイド

ドキュメントを執筆する際のガイドラインです。

## 基本的なMarkdown記法

### 見出し

```markdown
# 見出し1
## 見出し2
### 見出し3
```

### リスト

```markdown
- 項目1
- 項目2
  - サブ項目

1. 番号付き項目1
2. 番号付き項目2
```

### リンク

```markdown
[表示テキスト](URL)
[内部リンク](../index.md)
```

### コードブロック

````markdown
```python
def hello():
    print("Hello, World!")
```
````

## Material for MkDocs の機能

### アドモニション（注意書き）

```markdown
!!! note "タイトル"
    ノートの内容

!!! warning "警告"
    警告の内容

!!! tip "ヒント"
    ヒントの内容
```

結果：

!!! note "ノート"
    これはノートです。

!!! warning "警告"
    これは警告です。

!!! tip "ヒント"
    これはヒントです。

### タブ

```markdown
=== "Python"
    ```python
    print("Hello")
    ```

=== "JavaScript"
    ```javascript
    console.log("Hello");
    ```
```

=== "Python"
    ```python
    print("Hello")
    ```

=== "JavaScript"
    ```javascript
    console.log("Hello");
    ```

### テーブル

```markdown
| 列1 | 列2 | 列3 |
|-----|-----|-----|
| A   | B   | C   |
| D   | E   | F   |
```

| 列1 | 列2 | 列3 |
|-----|-----|-----|
| A   | B   | C   |
| D   | E   | F   |

## ファイル命名規則

- 小文字のみ使用
- スペースの代わりにハイフン `-` を使用
- 拡張子は `.md`

例：`getting-started.md`, `api-reference.md`

## 新しいページの追加

1. `docs/` 配下に `.md` ファイルを作成
2. `mkdocs.yml` の `nav` セクションに追加

```yaml
nav:
  - Home: index.md
  - 新しいページ: new-page.md
```
