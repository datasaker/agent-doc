# dsk-postgres-agent

`postgres agent`はデータベースの状態をリアルタイムで収集します。\
これにより、データベースのパフォーマンス指標、リソース使用量など、さまざまな情報を収集できます。\
収集したデータに基づいて、データベースのパフォーマンスのボトルネックを特定して対応できます。\
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## Supported version

|version|support|
|---|---|
|postgres 15|X|
|postgres 14|O|
|postgres 13|X|
|postgres 12|X|
|postgres 11|X|
|postgres 10|X|
|postgres 9|X|
|postgres 8|X|

## Datasaker先行作業を行いましたか？

現在の環境で `DataSaker`の先行操作が進行しなかった場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](README.md)

## Postgres agentのインストール

### 1. Postgres設定の変更

管理したいデータベースの `pg_stat_statements`モジュールが有効になっていることを確認してください。
[pg_stat_statementsリファレンスサイト]（https://www.postgresql.org/docs/14/pgstatstatements.html）

### 2. Postgres User 権限の設定

`postgres agent`をインストールするには、`postgres user`の権限が必要です。\
`postgres user`の権限を確認し、権限がない場合は権限を与えてください。\
必要なユーザー権限は次のとおりです。

- `SELECT`
- `UPDATE`
- `DELETE`
- `INSERT`

[postgres user権限参照サイト]（https://www.postgresql.org/docs/14/sql-grant.html）

### 3. パッケージのインストール
```shell
yum install dsk-postgres-agent
```
### 4. Postgresエージェントの設定
```shell
vi /etc/datasaker/dsk-postgres-agent/agent-config.yml
```
必要に応じて次の内容を修正します。

#### `agent-config.yml`
```yaml
agent:
  metadata:
    agent_name: dsk-postgres-agent
  option:
    exporter_config:
      command: "/usr/bin/dsk-postgres-exporter"
      port: 19187
      args:
        - --extend.query-path=/etc/datasaker/dsk-postgres-agent/queries.yaml
        - --data-source-user=<monitoring account name>
        - --data-source-pass=<monitoring account pass>
        - --data-source-uri=<monitoring database uri> # <ip>:<port>/dbname
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-postgres-agent
        url: localhost:19187
        filtering_configs:
          rule: drop
```
##### `metadata`
```yaml
# 에이전트 이름 (별칭)
[ agent_name: <string> | default = "dsk-postgres-agent" ]

# 관제 대상이 되는 환경이 어떤 클러스터로 묶여있는지에 대한 설정
[ cluster_id: <cluster_id> | default = "unknown" ]
```
各設定の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :-----------: | :------------: |
| `agent_name` |エージェント名（エイリアス）| dsk-postgres-agent | N / A |
| `cluster_id` |管理対象となる環境がどのクラスタにまとめられているかについての設定unknown | N / A |

##### `option.exporter_config.port`
```yaml
[ port: <uint16> | default = 19187 ]
```
各設定の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| ------------ | ---------------------------------------------------------------------------------------------------- | :-----------: | :------------: |
| `port` |エージェントが使用するポート番号既存の使用中のアプリケーションとポートの競合が発生したときに任意の値に変更する19187 | N / A |

##### `option.exporter_config.args`
```yaml
# 관제하려는 database의 접속권한을 가진 계정 정보와 주소를 입력합니다.
- --data-source-user=<monitoring account name>
- --data-source-pass=<monitoring account pass>
- --data-source-uri=<monitoring database uri> # <ip>:<port>/dbname
```
各 argument の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| ------------ | ---------------------------------------------------------------------------------------------------- | :-----------: | :------------: |
| `--data-source-user` |管理したいデータベースのアクセス権を持つアカウント情報を入力します。 | N / A | **✓** |
| `--data-source-pass` |管理したいデータベースのアクセス権を持つアカウントのパスワードを入力します。 | N / A | **✓** |
| `--data-source-uri` |管理するデータベースのアドレスを入力します。 | N / A | **✓** |

### 5. パッケージの実行
```shell
systemctl enable dsk-postgres-agent --now
```
### 6. パッケージ実行状態の確認
```shell
systemctl status dsk-postgres-agent
```
## Postgresエージェントを削除する

### 1. パッケージの中断
```shell
systemctl stop dsk-postgres-agent
```
### 2. パッケージの削除
```shell
yum remove dsk-postgres-agent
```
