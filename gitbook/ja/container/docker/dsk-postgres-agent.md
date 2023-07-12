#dsk-postgres-agent

## Docker環境にDataSaker PostgreSQLエージェントをインストールする

「Postgres agent」は「postgres-agent」と「plan-postgres-agent」で構成されています。
これにより、Postgresqlのパフォーマンス指標、リソース使用量、スロークエリなど、さまざまな情報を収集できます。
お客様のニーズに応じて、「plan-postgres-agent」は「on / off」の形式で使用できます。

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

## Agentの設定

Postgresエージェントは `postgresエージェント'と `plan-postgres-agent`で構成されています。

### postgres agent

`postgres agent`はデータベースの状態をリアルタイムで収集します。\
これにより、データベースのパフォーマンス指標、リソース使用量など、さまざまな情報を収集できます。\
収集したデータに基づいて、データベースのパフォーマンスのボトルネックを特定して対応できます。\
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

### plan-postgres agent

`plan-postgres-agent`はデータベースの `active session`をリアルタイムで収集します。
これにより、データベースのスロークエリに関する情報を収集できます。\
スロークエリを検出して、インデックスの作成、クエリの最適化などの方法でデータベースのパフォーマンスを向上させることができます。\
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在Docker環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](dsk-postgres-agent/ja/$%7BPREPARATION\_MANUAL\_JP%7D/)

## Postgres agentのインストール

### 1. Postgres エージェント設定の変更

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

### 3. Postgres agent 設定値登録

#### 必須入力事項のご案内

エージェントを接続するには、収集したいPostgreSQLサーバーのアドレス、データベース、ユーザーID、およびパスワードをエージェントに設定する必要があります。

「シェル
 DSK_PG_USER=<ユーザー>
 DSK_PG_SOURCE_PASS=<パスワード>
 DSK_PG_DB_NAME=<database>
 DSK_PG_HOST=<host>
 DSK_PG_PORT=<port>
「」

たとえば、アドレスが `192.168.123.132` で、デフォルトポート `5432` にサービス中の PostgreSQL を収集するためには端末に次のように設定できます。

「シェル
 DSK_PG_USER=postgres
 DSK_PG_SOURCE_PASS=postgres
 DSK_PG_DB_NAME=postgres
 DSK_PG_HOST=192.168.123.132
 DSK_PG_PORT=5432
「」

#### Postgres agent 設定値登録

次に、端末に次のコマンドを入力して、dsk-postgres-agentとdsk-plan-postgres-agentの設定ファイルを作成します。

「シェル
cd~
mkdir .datasaker
cat << EOF> ~/.datasaker/postgres-config.yml
agent:
  metadata:
    agent_name: dsk-postgres-agent
  option:
    exporter_config:
      command: /etc/datasaker/target-exporter
      port: 9187
    scrape_configs:
      - job_name: dsk-postgres-agent
        metrics_path: /metrics/short
        url: localhost:9187
      - job_name: dsk-postgres-agent-long
        scrape_interval: 60s
        scrape_timeout: 10s
        metrics_path: /metrics/long
        url: localhost:9187
        filtering_configs:
          rule: drop
EOF

cat << EOF> ~/.datasaker/plan-postgres-config.yml
agent:
  metadata:
    agent_name: dsk-plan-postgres-agent
  data_source_name:
    user: ${DSK_PG_USER}
    password: ${DSK_PG_SOURCE_PASS}
    address: ${DSK_PG_HOST}
    port: ${DSK_PG_PORT}
    DBName: ${DSK_PG_DB_NAME}
  explain:
    scrape_interval: 5s
    scrape_timeout: 5s
    slow_query_standard: 1s
EOF
「」

|設定値|説明
| ----------------------------- | ------------------------ |
| data\_source\_name.user | DBユーザー名|
| data\_source\_name.password | DBユーザーパスワード|
| data\_source\_name.address | PostgreSQLサーバー接続アドレス|
| data\_source\_name.port | PostgreSQLサーバー接続ポート|
| data\_source\_name.DBName | PostgreSQLデータベース|
| explain.scrape\_interval |活動セッションの収集サイクル|
| explain.scrape\_timeout |収集タイムアウト|
| explain.slow\_query\_standard |スロークエリ基準時間

#### Postgres agentのインストール

1. データセーカが使用するローカルディレクトリを作成します。

「シェル
 sudo mkdir -p /var/datasaker
 sudo chown -R datasaker:datasaker /var/datasaker/
「」

2. ドッカー命令をサーバに入力します。

「シェル
 DSK_PG_URI=${DSK_PG_HOST}:${DSK_PG_PORT}/${DSK_PG_DB_NAME}?sslmode=disable
 docker run -d --name dsk-postgres-agent\
  -v /var/datasaker/:/var/datasaker/\
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
  -v ~/.datasaker/postgres-config.yml:/etc/datasaker/dsk-postgres-agent/agent-config.yml:ro\
  -e DKS_LOG_LEVEL=info\
  -e DATA_SOURCE_USER=${DSK_PG_USER}\
  -e DATA_SOURCE_PASS=${DSK_PG_SOURCE_PASS}\-e DATA_SOURCE_URI=${DSK_PG_URI}\
  --restart=always\
  datasaker/dsk-postgres-agent
  docker run -d --name dsk-plan-postgres-agent\
    -v /var/datasaker/:/var/datasaker/\
    -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
    -v ~/.datasaker/plan-postgres-config.yml:/etc/datasaker/dsk-plan-postgres-agent/agent-config.yml:ro\
    -e DKS_LOG_LEVEL=info\
    --restart=always\
    datasaker/dsk-plan-postgres-agent
「」