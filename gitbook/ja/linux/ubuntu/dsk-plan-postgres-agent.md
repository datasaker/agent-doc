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

## DataSaker 先行作業を行いましたか？

現在のUbuntu環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker 先行操作]($%7BPREPARATION\_MANUAL\_KR%7D/)


## Plan Postgres Agent install

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

### 4. Postgres agent 設定値の登録

#### 必須入力項目

必須入力項目は次のとおりです。 Postgres設定に合わせて値を入れてください

|エンティティ説明|
| --------------------------------- | -------------------------- |
| agent.data\_source\_name.user | Postgres user IDを入力します。 |
| agent.data\_source\_name.password | Postgres user パスワードを入力します。 |
| agent.data\_source\_name.address | Postgresアドレスを入力してください。 |
| agent.data\_source\_name.port | Postgresポートを入力してください。 |
| agent.data\_source\_name.DBName | Postgresデータベースを入力します。 |

#### オプション入力

```shell
vi /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml
```

必要に応じて次の内容を修正します。

`` yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent" # エージェント名 (エイリアス) default=dsk-plan-postgres-agent
  data_source_name:
    user: 'user'
    password: 'pass'
    address: '127.0.0.1'
    port: '5432'
    DBName: 'database'
  explain:
    scrape_interval: 30s
    scrape_timeout: 5s
    slow_query_standard: 5s
    executor_number: 10
    sender_number: 10
    activity_query_buffer: 50
    plan_sender_buffer: 50
```

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

\
\


## Plan Postgres Agentを削除する

### 1. パッケージの中断

```shell
systemctl stop dsk-plan-postgres-agent
```

### 2. パッケージの削除

```shell
sudo apt remove dsk-plan-postgres-agent
```