#dsk-plan-postgres-agent

`plan-postgres-agent`はデータベースの `active session`をリアルタイムで収集します。
これにより、データベースのスロークエリに関する情報を収集できます。\
スロークエリを検出して、インデックスの作成、クエリの最適化などの方法でデータベースのパフォーマンスを向上させることができます。\
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。\
\


## Supported version

| version |サポート|
| ----------- | ------- |
| postgres 15 | X |
| postgres 14 | O |
| postgres 13 | X |
| postgres 12 | X |
| postgres 11 | X |
| postgres 10 | X |
| postgres 9 | X |
| postgres 8 | X |

\
\


## DataSaker 先行作業を行いましたか？

現在、Amazon Linux 2環境では、`DataSaker`の先行操作が進行していない場合は、`DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作]($%7BPREPARATION\_MANUAL\_KR%7D/)\
\


## Plan Postgres Agent Install

### 1. Postgres User権限の設定

`plan-postgres-agent`をインストールするには、`postgres user`の権限が必要です。\
`postgres user`の権限を確認し、権限がない場合は権限を与えてください。\
必要なユーザー権限は次のとおりです。

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[postgres user権限参照サイト]（https://www.postgresql.org/docs/14/sql-grant.html）

### 2. パッケージのインストール

「シェル
yum install dsk-plan-postgres-agent
「」

### 3. Plan Postgres Agent の設定

「シェル
vi /etc/datasaker/dsk-plan-postgres-agent/agent-config.yml
「」

必要に応じて次の内容を修正します。

#### `agent-config.yml`

`` yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent" # エージェント名 (エイリアス) default=dsk-plan-postgres-agent
  data_source_name:
    user: # <user_name>
    password: # <user_password>
    address: # <database_address>
    port: # <database_port>
    DBName: # <database_name>
  explain:
    scrape_interval: 30s # <activity_session_scrape_time>
    scrape_timeout: 5s # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s # <slow_query_standard>
    executor_number: 10 # <explain executor number>
    sender_number: 10 # <explain sender number>
    activity_query_buffer: 50 # <activity query buffer>
    plan_sender_buffer: 50 # <explain result buffer>
「」

**`metadata`**

`` yaml
# エージェント名(エイリアス)
[ agent_name: <string> | default = "dsk-trace-agent" ]

＃管理対象となる環境がどのクラスタにまとめられているかについての設定
[cluster_id：<cluster_id> | default = "unknown" ]
「」

**`data_source_name`**

`` yaml
#postgresアカウント名
[ user: <string> | required ]
#postgresアカウントパスワード
[パスワード：<文字列> | required ]
#postgresサーバurl
[ address: <string> | required ]
# postgres サーバー port
[ port: <uint16> | | required ]
#postgresサーバデータベース名
[ DBName: <string> | required ]
「」

**`explain`**

`` yaml
# activity session scrape サイクル
[scrape_interval：<seconds> | default=30s]
#activity session scrape queryのタイムアウト時間
[scrape_timeout：<seconds> | default=5s]
#slow query基準
[slow_query_standard：<seconds> | default=5s ]
# explainを実行するためのスレッド数
[executor_number：<int8> | | default=10 ]
# explain 結果を送信するスレッド数
[sender_number：<int8> | | default=10 ]
#activity query buffer
[activity_query_buffer：<int16> | default=50 ]
#explain result buffer
[plan_sender_buffer：<int16> | | default=50 ]
「」

### 4. パッケージの実行

「シェル
systemctl enable dsk-plan-postgres-agent --now
「」

### 5. パッケージ実行状態の確認

「シェル
systemctl status dsk-plan-postgres-agent
「」

## Plan Postgres Agentを削除する

### 1. パッケージの中断

「シェル
systemctl stop dsk-plan-postgres-agent
「」

### 2. パッケージの削除

「シェル
yum remove dsk-plan-postgres-agent
「」