---
tags:
  - 手動作成
---

# 選挙データの作り方

Streamlit管理画面の「選挙管理」ページから手動で作成します。

選挙は、地方議会の「第n期」を決定する選挙を表すデータです。開催主体（GoverningBody）に紐付き、会議体（Conference）がどの選挙で決まった期かを明確にします。

## 入力プロパティ

| フィールド | 必須 | 説明 |
|------------|------|------|
| 開催主体 | はい | 紐付ける開催主体を選択 |
| 期番号 | はい | 期番号（例: 21） |
| 選挙実施日 | はい | 選挙が行われた日付 |
| 選挙種別 | いいえ | 選挙の分類（統一地方選挙、通常選挙、補欠選挙など） |

## 他オブジェクトとのリレーション

```mermaid
erDiagram
    GoverningBody-開催主体 ||--o{ Election-選挙 : "実施"
    Election-選挙 ||--o{ Conference-会議体 : "決定"

    GoverningBody-開催主体 {
        int id
        string name
        string type
        string organization_code
        string organization_type
    }

    Election-選挙 {
        int id
        int governing_body_id
        int term_number
        date election_date
        string election_type
    }

    Conference-会議体 {
        int id
        string name
        int governing_body_id
        int election_id
        string term
        string prefecture
        string members_introduction_url
    }
```

### リレーションの説明

| 関連テーブル | 関係 | 説明 |
|-------------|------|------|
| **GoverningBody（開催主体）** | 選挙 has one 開催主体 | この選挙を実施した自治体です |
| **Conference（会議体）** | 選挙 has many 会議体 | この選挙によって決まった期に属する会議体です |

## 用途

選挙データは以下の目的で使用されます：

- 会議体が「いつ行われた選挙で決まった期か」を明確にする
- 同じ期番号でも選挙日が異なる場合を区別する
- 統一地方選挙や補欠選挙など、選挙種別による分析を可能にする
