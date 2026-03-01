---
tags:
  - 手動作成
  - シードデータ作成済み
---

# 会派（議員団）データの作り方

Streamlit管理画面の「議員団管理」ページから手動で作成します。

会派（議員団）は、議会内で政治家が活動するためのグループです。開催主体（GoverningBody）に紐付きます。

### 会派と政党の関係について

日本の地方議会では、会派（議員団）と政党は必ずしも1対1ではありません。例えば「自民党・無所属の会」のように複数政党の議員が合同で会派を組むケースがあります。

会派と政党の対応関係は `parliamentary_group_parties` 中間テーブルで管理されます（詳細は[政党データの作り方](political-party.md)を参照）。`is_primary` フラグで主要政党を識別します。

従来の `political_party_id` による直接紐付けも引き続きサポートしていますが、複数政党が合同する会派の場合は中間テーブルを使用してください。

## 入力プロパティ

| フィールド | 必須 | 説明 |
|------------|------|------|
| 所属開催主体 | はい | 紐付ける開催主体を選択 |
| 議員団名 | はい | 会派の名称（例: 自民党市議団） |
| 議員団URL | いいえ | 会派の公式ページのURL |
| 説明 | いいえ | 会派の説明や特徴 |
| 活動中 | はい | 活動中かどうか（デフォルト: 活動中） |
| 政党 | いいえ | 紐付ける政党（会派自動紐付けに使用） |

## 他オブジェクトとのリレーション

```mermaid
erDiagram
    GoverningBody-開催主体 ||--o{ ParliamentaryGroup-会派 : "所属"
    ParliamentaryGroup-会派 ||--o{ ParliamentaryGroupMembership-会派所属 : "所属メンバー"
    ParliamentaryGroup-会派 ||--o{ ParliamentaryGroupParty-会派政党対応 : "対応"
    ParliamentaryGroup-会派 ||--o{ ExtractedParliamentaryGroupMember-抽出済み会派メンバー : "抽出"
    ParliamentaryGroup-会派 ||--o{ ProposalSubmitter-議案提出者 : "提出元"
    ParliamentaryGroup-会派 ||--o{ ProposalParliamentaryGroupJudge-会派別議案賛否 : "投票"
    PoliticalParty-政党 ||--o{ ParliamentaryGroupParty-会派政党対応 : "対応"

    GoverningBody-開催主体 {
        int id
        string name
        string type
    }

    ParliamentaryGroup-会派 {
        int id
        string name
        int governing_body_id
        int political_party_id
        string url
        string description
        bool is_active
    }

    ParliamentaryGroupParty-会派政党対応 {
        int id
        int parliamentary_group_id
        int political_party_id
        bool is_primary
    }

    PoliticalParty-政党 {
        int id
        string name
    }

    ParliamentaryGroupMembership-会派所属 {
        int id
        int politician_id
        int parliamentary_group_id
        date start_date
        date end_date
        string role
    }

    ExtractedParliamentaryGroupMember-抽出済み会派メンバー {
        int id
        int parliamentary_group_id
        string extracted_name
        int matched_politician_id
    }

    ProposalSubmitter-議案提出者 {
        int id
        int proposal_id
        int parliamentary_group_id
    }

    ProposalParliamentaryGroupJudge-会派別議案賛否 {
        int id
        int proposal_id
        string judgment
    }
```

### リレーションの説明

| 関連テーブル | 関係 | 説明 |
|-------------|------|------|
| **GoverningBody（開催主体）** | 会派 has one 開催主体 | この会派が所属する開催主体です(`自由民主党京都市会議員団`のように開催主体に対して会派が紐づきます。) |
| **ParliamentaryGroupMembership（会派所属）** | 会派 has many 会派所属 | この会派に所属する政治家の一覧です。期間・役割付きで記録されます |
| **ExtractedParliamentaryGroupMember（抽出済み会派メンバー）** | 会派 has many 抽出済み会派メンバー | 外部Webページから抽出された会派メンバー情報です。政治家との自動マッチングに使用されます |
| **ProposalSubmitter（議案提出者）** | 会派 has many 議案提出者 | この会派が提出元となっている議案です(議案は会派として提出するケースがあります。) |
| **ProposalParliamentaryGroupJudge（会派別議案賛否）** | 会派 has many 会派別議案賛否 | 会派単位での議案に対する賛否を記録します(議案は会派単位で賛否を表明するケースがあります。) |
| **ParliamentaryGroupParty（会派政党対応）** | 会派 has many 会派政党対応 | 会派と政党の多対多の対応関係です。`is_primary` で主要政党を識別します。SEEDファイル: `seed_parliamentary_group_parties_generated.sql` |
| **PoliticalParty（政党）** | 会派 has one 政党（任意、レガシー） | 単一政党との直接紐付けです。複数政党が合同する会派では `ParliamentaryGroupParty` を使用してください |

!!! tip "リレーション"
    - 会派メンバーシップ（ParliamentaryGroupMembership）のデータ構造・自動紐付け・SEED生成・パイプライン品質検証については、[会派メンバーシップ](relations/parliamentary-group-membership.md)を参照してください。
    - 会派-政党マッピング調査については、[会派-政党対応](relations/parliamentary-group-party.md)を参照してください。
