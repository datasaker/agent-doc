# dsk-plan-postgres-agent

`plan-postgres-agent`はデータベースの `active session`をリアルタイムで収集します。
これにより、データベースのスロークエリに関する情報を収集できます。\
スロークエリを検出して、インデックスの作成、クエリの最適化などの方法でデータベースのパフォーマンスを向上させることができます。\
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。\

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

## DataSaker先行作業を進めましたか？

現在のUbuntu環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作]($%7BPREPARATION\_MANUAL\_KR%7D/)

## Plan Postgres Agentのインストール

### 1. Postgres設定の変更

管理したいデータベース `pg_stat_statements` モジュールが有効になっていることを確認してください。
[pg\_stat\_statementsリファレンスサイト]（https://www.postgresql.org/docs/14/pgstatstatements.html）

### 2. Postgres User 権限の設定

`postgres agent`をインストールするには、`postgres user`の権限が必要です。\
`postgres user`の権限を確認し、権限がない場合は権限を与えてください。\
必要なユーザー権限は次のとおりです。

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[postgres user権限参照サイト]（https://www.postgresql.org/docs/14/sql-grant.html）

### 3. パッケージのインストール
```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-plan-postgres-agent
```
### 4. Plan Postgres Agent の設定
```shell
vi /etc/datasaker/dsk-plan-postgres-agent/agent-config.yml
```
必要に応じて次の内容を修正します。

#### `agent-config.yml`
```yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent" # <agent_alias_name> default=dsk-plan-postgres-agent
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
```
各設定の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent.metadata.agent_name` |エージェント名（エイリアス）| dsk-plan-postgres-agent | **✓** |
| `agent.data_source_name.user` | postgresアカウント名| N / A | **✓** |
| `agent.data_source_name.password` | postgresアカウントパスワード| N / A | **✓** |
| `agent.data_source_name.address` | postgresサーバーurl | N / A | **✓** |
| `agent.data_source_name.port` | postgresサーバーポート| N / A | **✓** |
| `agent.data_source_name.DBName` | postgresサーバーデータベース名| N / A | **✓** |
| `agent.explain.scrape_interval` | activity session scrapeサイクル| 30秒| |
| `agent.explain.scrape_timeout` | activity session scrape queryのタイムアウト時間| 5秒| |
| `agent.explain.slow_query_standard` |スロークエリ基準| 5秒| |
| `agent.explain.executor_number` | explainを実行するためのスレッド数10 | |
| `agent.explain.sender_number` | explain結果を送信するスレッドの数| 10 | |
| `agent.explain.activity_query_buffer` | activity query buffer | 50 | |
| `agent.explain.plan_sender_buffer` | explain result buffer | 50 | |

### 5. パッケージの実行
```shell
systemctl enable dsk-plan-postgres-agent --now
```
### 6. パッケージ実行状態の確認
```shell
systemctl status dsk-plan-postgres-agent
```
または
```shell
serivce dsk-plan-postgres-agent
```
## Plan Postgres Agentを削除する

### 1. パッケージの中断
```shell
systemctl stop dsk-plan-postgres-agent
```
### 2. パッケージの削除
```shell
sudo apt remove dsk-plan-postgres-agent
```
