#dsk-postgres-agent

## Kubernetes環境でDataSaker Postgresエージェントをインストールする

「Postgres agents」は「postgres-agent」と「plan-postgres-agent」で構成されています。
これにより、データベースのパフォーマンス指標、リソース使用量、スロークエリなど、さまざまな情報を収集できます。\
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

Postgresエージェントは `postgresエージェント'と `plan-postgresエージェント'で構成されています。

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

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](dsk-postgres-agent/ja/$%7BPREPARATION\_MANUAL\_JP%7D/)


## Postgres agentのインストール

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

### 3. Postgres agent 設定値登録

#### 必須入力項目

必須入力項目は次のとおりです。 Postgres設定に合わせて値を入れてください

|エンティティ説明|
| --------------------------------- | -------------------------------- |
| postgresAgents.list\[].name | Postgres区別できる名前を付けます。 |
| postgresAgents.list\[].targetAddr | Postgresにtarget addressと入力します。 |
| postgresAgents.list\[].targetPort | Postgres target portと入力します。 |
| postgresAgents.list\[].database | Postgresデータベース名を入力します。 |
| postgresAgents.list\[].user | Postgres user IDを入力します。 |
| postgresAgents.list\[].pass | Postgres user パスワードを入力します。 |

#### オプション入力

「シェル
cat << EOF >> ~/datasaker/config.yaml

postgresAgents:
  list:
    - name: 'my-postgres'
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19187
      tolerations: []
      targetAddr: '127.0.0.1'
      targetPort: '5432'
      database: "database"
      user: 'user'
      pass: "pass"
      exporterArgs: []
      postgresPlan: true
      explain:
        scrape_interval: 5s
        scrape_timeout: 5s
        slow_query_standard: 5s
        executor_number: 10
        sender_number: 10
      resources:
        requests:
          cpu: 100 m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
「」

### 4. Postgresエージェントを有効にする

「シェル
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
「」