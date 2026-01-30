# 政治家データの作り方

Streamlit管理画面の「政治家管理」ページから手動で作成します。

### 手動作成としている理由

当初はLLMを使って各政党の議員一覧ページから政治家データを自動抽出する実装を進めていましたが、以下の理由から手動作成に切り替えました。

- **データの信頼性要件が高い**: 政治家データは発言者の名寄せ・会派所属・議案賛否など多数のテーブルから参照される中心的なデータであり、誤りが広範囲に影響するため、正確性が求められる
- **自動抽出のコストが見合わない**: 政党ごとに議員一覧ページの構成・掲載情報・情報の持ち方（画像に埋め込まれている等）が大きく異なり、個別対応のコストが人手で登録するコストを上回ると判断した

## 作成の流れ

政治家データの作成には、大きく2つの経路があります。

### 1. 政党の議員一覧ページから網羅的に作成する

各政党の公式サイトにある議員一覧ページを参照し、選挙区ごとに地方議員を含めて網羅的に登録します。

ただし、政党ページには市議会議員が掲載されていないケースがあります。その場合は、各市区町村レベルでその政党の議員団が存在するかを検索し、見つかった議員を追加で登録します。

### 2. 紐付け処理の中で都度作成する

発言者の名寄せや会派メンバーのマッチングなど、各オブジェクトとの紐付け処理を行う際に、紐付け先の政治家が存在しない場合があります。その場合は画面上に政治家の新規作成フォームが表示され、その場で作成して紐付けを完了できます。

## 入力プロパティ

| フィールド | 必須 | 説明 |
|------------|------|------|
| 名前 | はい | 政治家の氏名 |
| 選挙区の都道府県 | はい | 47都道府県または「比例代表」から選択 |
| 政党 | いいえ | 「無所属」または登録済みの政党から選択 |
| 選挙区 | はい | 選挙区名（例: 東京1区） |
| プロフィールURL | いいえ | 政治家のプロフィールページのURL |

## 他オブジェクトとのリレーション

```mermaid
erDiagram
    PoliticalParty-政党 ||--o{ Politician-政治家 : "所属"
    Politician-政治家 ||--o{ Speaker-発言者 : "名寄せ"
    Politician-政治家 ||--o{ ParliamentaryGroupMembership-会派所属 : "所属"
    Politician-政治家 ||--o{ PoliticianAffiliation-会議体所属 : "所属"
    Politician-政治家 ||--o{ ProposalJudge-議案賛否 : "投票"
    Politician-政治家 ||--o{ PoliticianOperationLog-操作ログ : "記録"
    Politician-政治家 ||--o{ ExtractedConferenceMember-抽出済み会議体メンバー : "マッチング"
    Politician-政治家 ||--o{ ExtractedParliamentaryGroupMember-抽出済み会派メンバー : "マッチング"
    Politician-政治家 ||--o{ ExtractedProposalJudge-抽出済み議案賛否 : "マッチング"
    Speaker-発言者 ||--o{ Conversation-発言 : "発言元"
    ParliamentaryGroup-会派 ||--o{ ParliamentaryGroupMembership-会派所属 : "所属先"
    Conference-会議体 ||--o{ PoliticianAffiliation-会議体所属 : "所属先"
    Conference-会議体 ||--o{ ParliamentaryGroup-会派 : "配下"
    Proposal-議案 ||--o{ ProposalJudge-議案賛否 : "対象"

    Politician-政治家 {
        int id
        string name
        string prefecture
        string district
        int political_party_id
        string furigana
        string profile_page_url
        string party_position
    }

    PoliticalParty-政党 {
        int id
        string name
        string members_list_url
    }

    Speaker-発言者 {
        int id
        int politician_id
        string name
    }

    Conversation-発言 {
        int id
        int speaker_id
        string content
    }

    ParliamentaryGroupMembership-会派所属 {
        int id
        int politician_id
        int parliamentary_group_id
    }

    ParliamentaryGroup-会派 {
        int id
        string name
        int conference_id
    }

    PoliticianAffiliation-会議体所属 {
        int id
        int politician_id
        int conference_id
    }

    ProposalJudge-議案賛否 {
        int id
        int politician_id
        int proposal_id
        int politician_party_id
    }

    PoliticianOperationLog-操作ログ {
        int id
        int politician_id
        string operation_type
    }

    ExtractedConferenceMember-抽出済み会議体メンバー {
        int id
        int matched_politician_id
    }

    ExtractedParliamentaryGroupMember-抽出済み会派メンバー {
        int id
        int matched_politician_id
    }

    ExtractedProposalJudge-抽出済み議案賛否 {
        int id
        int matched_politician_id
    }

    Conference-会議体 {
        int id
        string name
    }

    Proposal-議案 {
        int id
        string title
    }
```

### リレーションの説明

| 関連テーブル | 関係 | 説明 |
|-------------|------|------|
| **PoliticalParty（政党）** | 政治家 → 政党 | 政治家が所属する政党を表します。「無所属」の場合は未設定です |
| **Speaker（発言者）** | 政治家 ← 発言者 | 議事録から抽出された発言者を政治家と名寄せして紐付けます。自動マッチング（ルールベース / LLM）と手動紐付けの両方に対応しています |
| **Conversation（発言）** | 発言者 ← 発言 | 議事録内の個々の発言です。発言者を経由して、どの政治家の発言かを間接的に辿れます |
| **ParliamentaryGroupMembership（会派所属）** | 政治家 → 会派 | 政治家がどの会派（議員団）に所属しているかを期間・役割付きで記録します |
| **PoliticianAffiliation（会議体所属）** | 政治家 → 会議体 | 政治家がどの会議体（議会・委員会）に所属しているかを期間・役割付きで記録します |
| **ProposalJudge（議案賛否）** | 政治家 → 議案 | 政治家が議案に対してどう投票したか（賛成・反対・棄権・欠席）を記録します |
| **PoliticianOperationLog（操作ログ）** | 政治家 ← ログ | 政治家レコードの作成・更新・削除の操作履歴を監査目的で記録します |
| **ExtractedConferenceMember（抽出済み会議体メンバー）** | 政治家 ← 抽出データ | 外部Webページから抽出された会議体メンバー情報を政治家と自動マッチングした結果です。マッチング成功後に PoliticianAffiliation へ変換されます |
| **ExtractedParliamentaryGroupMember（抽出済み会派メンバー）** | 政治家 ← 抽出データ | 外部Webページから抽出された会派メンバー情報を政治家と自動マッチングした結果です。マッチング成功後に ParliamentaryGroupMembership へ変換されます |
| **ExtractedProposalJudge（抽出済み議案賛否）** | 政治家 ← 抽出データ | 外部Webページから抽出された議案賛否情報を政治家と自動マッチングした結果です。マッチング成功後に ProposalJudge へ変換されます |
