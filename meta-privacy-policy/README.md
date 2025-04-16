# プライバシーポリシー

弊チームが提供する Meta のデータ取得サービスについてのプライバシーポリシーです。

## 処理するデータの種類

1. 広告の insight データ

2. ad, adset, campaign のマスタデータ

3. ビジネスマネージャに紐づく account のマスタデータ

## 処理方法

```mermaid
sequenceDiagram
Autonumber
participant GCW as Google Cloud Workflows
participant CF as Cloud Functions
participant BQ as BigQuery
participant M as Meta
participant DBT as dbt
participant LS as Looker Studio
GCW->>CF: エンドポイントにアクセス
rect rgb(100, 100, 100)
Note right of CF: アクセストークンの取得・更新
CF->>BQ: アクセストークン取得
BQ-->>CF: アクセストークン
CF->>M: アクセストークン更新リクエスト
M-->>CF: 新アクセストークン
CF->>BQ: 新アクセストークンを更新
end

rect rgb(100, 100, 100)
Note right of CF: Accounts インジェスチョン
CF->>M: アカウントデータ取得
M-->>CF: アカウントデータ
CF->>BQ: アカウントデータインサート
end
loop account_ids
rect rgb(100, 100, 100)
Note right of CF: Insights インジェスチョン
CF->>M: インサイトデータ取得
M-->>CF: インサイトデータ
CF->>BQ: インサイトデータインサート
end
rect rgb(100, 100, 100)
Note right of CF: AdEntity（広告関連データ） インジェスチョン
loop [Campaigns, AdSets, Ads]
CF->>M: ACTIVEなデータ取得
M-->>CF: ACTIVEなデータ
CF->>M: 最近updateされたデータ取得
M-->>CF: 最近updateされたデータ
CF->>CF: 広告関連データ加工
end
end
end

DBT->>BQ: データ加工
BQ-->>LS: レポート表示

```

## 処理の目的

取得したデータを用いて、Looker Studio でレポートを作成する。

## データ削除の依頼方法

n.hori@androots.co.jp にお問合せください。