# dsk-postgres-agent

`postgres agent`はデータベースの状態をリアルタイムで収集します。\
これにより、データベースのパフォーマンス指標、リソース使用量など、さまざまな情報を収集できます。\
収集したデータに基づいて、データベースのパフォーマンスのボトルネックを特定して対応できます。\
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。


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

### 3. パッケージのインストール

``` bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres-agent
```

### 4. agent-configの設定

`/etc/datasaker/dsk-postgres-agent/agent-config.yml`に内容を書き込みます。

``` yaml
agent:
  metadata:
    agent_name: dsk-postgres-agent # エージェント名 (エイリアス) default=dsk-postgres-agent
  option:
    exporter_config:
      command: "/usr/bin/dsk-postgres-exporter"
      port: 19187
      args:
        - --extend.query-path=/etc/datasaker/dsk-postgres-agent/queries.yaml
        - --data-source-user=<monitoring account name>
        - --data-source-pass=<monitoring account pass>
        - --data-source-uri=<monitoring database uri>#<ip>:<port>/dbname
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-postgres-agent
        url: localhost:19187
        filtering_configs:
          rule: drop
```

### 5. パッケージの実行

```shell
systemctl enable dsk-postgres-agent --now
```

### 6. パッケージ実行状態の確認

```shell
systemctl status dsk-postgres-agent
```

または

```shell
service dsk-postgres-agent
```

## Postgresエージェントを削除する

### 1. パッケージの中断

```shell
systemctl stop dsk-postgres-agent
```

### 2. パッケージの削除

```shell
sudo apt remove dsk-postgres-agent
```