# Coming Soon
<!--
# Ubuntu環境にDataSaker Mysql agentをインストールする(Beta)
`Mysql agent`は、データベースの状態とスロークエリをリアルタイムで収集します。
これにより、データベースのパフォーマンス指標、リソース使用量、スロークエリなど、さまざまな情報を収集できます。
収集されたデータに基づいて、データベースのパフォーマンスのボトルネックを特定し、対応できます。
さらに、スロークエリを検出して、インデックスの作成、クエリの最適化などの方法でデータベースのパフォーマンスを向上させることができます。
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。
<br><br>

#Supported version
|version|support|
|---|---|
|MySQL 8.0.33|O|

<br><br>

＃DataSaker先行作業を行いましたか？
現在のUbuntu環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker先行操作](README.md)
<br><br>

# Mysql agentのインストール
## 1. MySQL設定の変更
管理したいデータベース `performance_schema=ON` モジュールがアクティブであることを確認してください。
[performance_schemaリファレンスサイト]（https://dev.mysql.com/doc/refman/8.0/en/performance-schema-quick-start.html）

## 2. MySQL User権限の設定
`MySQL agent`をインストールするには、 `MySQL User`に権限を付与する必要があります。\
`MySQL user`の権限を確認し、権限がない場合は権限を付与してください。
必要なユーザー権限は次のとおりです。
- `SELECT`
- `UPDATE`
- `DELETE`
- `INSERT`

[MySQL user権限参照サイト](https://dev.mysql.com/doc/refman/8.0/en/grant.html)

## 3. パッケージのインストール
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mysql-agent
```
## 4. MySQL agent 設定値登録
### 環境変数の登録
```
env DATA_SOURCE_NAME=아이디:패스워드@MySQL아이피:포트
```
### オプション入力
`/etc/datasaker/dsk-mysql-agent/agent-config.yml`に内容を書き込みます。
```yaml
agent:
  metadata:
    agent_name: 'dsk-mysql-agent' # 에이전트 이름 (별칭) default=dsk-node-agent
  option:
    exporter_config:
      command: "/usr/bin/dsk-mysqld-exporter"
      port: 19104
      args:
        - --collect.info_schema.clientstats
        - --collect.info_schema.innodb_metrics
        - --collect.info_schema.innodb_tablespaces
        - --collect.info_schema.innodb_cmp
        - --collect.info_schema.innodb_cmpmem
        - --collect.info_schema.processlist
        - --collect.info_schema.query_response_time
        - --collect.info_schema.replica_host
        - --collect.info_schema.tables
        - --collect.info_schema.tables.databases=‘*’
        - --collect.info_schema.tablestats
        - --collect.info_schema.schemastats
        - --collect.info_schema.userstats
        - --collect.perf_schema.eventsstatements
        - --collect.perf_schema.eventsstatementssum
        - --collect.perf_schema.eventswaits
        - --collect.perf_schema.file_events
        - --collect.perf_schema.file_instances
        - --collect.perf_schema.file_instances.remove_prefix=false
        - --collect.perf_schema.indexiowaits
        - --collect.perf_schema.memory_events
        - --collect.perf_schema.memory_events.remove_prefix=false
        - --collect.perf_schema.tableiowaits
        - --collect.perf_schema.tablelocks
        - --collect.perf_schema.replication_group_members
        - --collect.perf_schema.replication_group_member_stats
        - --collect.perf_schema.replication_applier_status_by_worker
    scrape_configs:
      - job_name: 'dsk-mysql-agent'
        url: localhost:19104                                              # 
        filtering_configs:
          rule: drop
```
## 5. パッケージの実行
```bash
$ sudo -E dsk-mysql-agent start
Agent is running
```
## 6. パッケージ実行状態の確認
### Running
```bash
$ sudo dsk-mysql-agents status
Agent is running
```
### Not Running
```bash
$ sudo dsk-mysql-agents status
Agent is not running
```
<br><br>

# Mysql agentを削除する
## 1. パッケージの中断
```bash
sudo dsk-mysql-agent stop
```
## 2. パッケージの削除
```bash
sudo apt remove dsk-mysql-agent
```
-->