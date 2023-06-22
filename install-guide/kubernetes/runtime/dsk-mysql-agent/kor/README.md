# Coming soon
<!--
# 쿠버네티스 환경에서 DataSaker MySQL agents 설치하기
'MySQL agents'는 `mysql agent`와 `plan-mysql-agent`로 구성되어 있습니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.\
고객의 요구사항에 따라 `plan-mysql-agent`는 `on/off` 형태로 사용 할 수 있습니다.
<br><br>

# Supported version
|version|support|
|---|---|
|MySQL 8.0.33|O|

<br><br>

# Agent 구성
## mysql agent
`mysql agent`는 데이터베이스의 상태를 실시간으로 수집합니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량 등 다양한 정보를 수집할 수 있습니다.\
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## plan-mysql agent
`plan-mysql-agent`는 데이터베이스의 `active session`을 실시간으로 수집합니다.\
이를 통해 데이터베이스의 슬로우 쿼리에 대한 정보를 수집할 수 있습니다.\
슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.
<br><br>

# DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})
<br><br>

# Mysql agent install
## 1. MySQL 설정 변경
관제하려는 데이터베이스 `performance_schema=ON` 모듈의 활성화 된 상태인지 확인 부탁드립니다.\
[performance_schema 참조사이트](https://dev.mysql.com/doc/refman/8.0/en/performance-schema-quick-start.html)

## 2. MySQL User 권한 설정
`MySQL agent`를 설치하기 위해서는 `MySQL User`에 권한을 부여해야 합니다.\
`MySQL user`의 권한을 확인하고, 권한이 없다면 권한을 부여해주세요.\
필요한 User 권한은 다음과 같습니다.
- `SELECT`
- `UPDATE`
- `DELETE`
- `INSERT`

[MySQL user 권한 참조사이트](https://dev.mysql.com/doc/refman/8.0/en/grant.html)

## 3. Mysql agent 설정값 등록
### 필수입력 항목
필수입력 항목은 다음과 같습니다. MySQL 설정에 맞게 값을 넣어주세요
|Entity|Describe|
|---|---|
|mysqlAgents.list[].name|MySQL을 구분할 수 있는 명칭을 부여합니다.|
|mysqlAgents.list[].targetAddr|MySQL을 target address를 입력합니다.|
|mysqlAgents.list[].targetPort|MySQL을 target port를 입력합니다.|
|mysqlAgents.list[].database|MySQL Database명을 입력합니다.|
|mysqlAgents.list[].user|MySQL user 아이디를 입력합니다.|
|mysqlAgents.list[].pass|MySQL user 패스워드를 입력합니다.|

### 옵션입력
```shell
cat << EOF >> ~/datasaker/config.yaml

mysqlAgents:
  list:
    - name: 'my-mysql'
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19104
      tolerations: []
      targetAddr: '127.0.0.1'
      targetPort: '3306'
      database: "database"
      user: 'user'
      pass: "pass"
      exporterArgs: []
      extraArgs: []
      mysqlPlan: true
      explain:
        scrape_interval: 5s
        scrape_timeout: 5s
        slow_query_standard: 5s
        executor_number: 10
        sender_number: 10
        activity_query_buffer: 50
        plan_sender_buffer: 50
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
```

## 4. Mysql agent 활성화
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
<br><br>

# exporterArgs value
`DataSaker MySQL 에이전트`의 인자 목록입니다.

| Argument                                                  | MySQL Version | Description                                                                           |
|-----------------------------------------------------------|---------------|---------------------------------------------------------------------------------------|
| collect.info_schema.clientstats                           | 5.5           | If running with userstat=1, set to true to collect client statistics.                 |
| collect.info_schema.innodb_metrics                        | 5.6           | Collect metrics from information_schema.innodb_metrics.                               |
| collect.info_schema.innodb_tablespaces                    | 5.7           | Collect metrics from information_schema.innodb_sys_tablespaces.                       |
| collect.info_schema.innodb_cmp                            | 5.5           | Collect InnoDB compressed tables metrics from information_schema.innodb_cmp.          |
| collect.info_schema.innodb_cmpmem                         | 5.5           | Collect InnoDB buffer pool compression metrics from information_schema.innodb_cmpmem. |
| collect.info_schema.processlist                           | 5.1           | Collect thread state counts from information_schema.processlist.                      |
| collect.info_schema.processlist.min_time                  | 5.1           | Minimum time a thread must be in each state to be counted. (default: 0)               |
| collect.info_schema.query_response_time                   | 5.5           | Collect query response time distribution if query_response_time_stats is ON.          |
| collect.info_schema.replica_host                          | 5.6           | Collect metrics from information_schema.replica_host_status.                          |
| collect.info_schema.tables                                | 5.1           | Collect metrics from information_schema.tables.                                       |
| collect.info_schema.tables.databases                      | 5.1           | The list of databases to collect table stats for, or '*' for all.                     |
| collect.info_schema.tablestats                            | 5.1           | If running with userstat=1, set to true to collect table statistics.                  |
| collect.info_schema.schemastats                           | 5.1           | If running with userstat=1, set to true to collect schema statistics                  |
| collect.info_schema.userstats                             | 5.1           | If running with userstat=1, set to true to collect user statistics.                   |
| collect.perf_schema.eventsstatements                      | 5.6           | Collect metrics from performance_schema.events_statements_summary_by_digest.          |
| collect.perf_schema.eventsstatements.digest_text_limit    | 5.6           | Maximum length of the normalized statement text. (default: 120)                       |
| collect.perf_schema.eventsstatements.limit                | 5.6           | Limit the number of events statements digests by response time. (default: 250)        |
| collect.perf_schema.eventsstatements.timelimit            | 5.6           | Limit how old the 'last_seen' events statements can be, in seconds. (default: 86400)  |
| collect.perf_schema.eventsstatementssum                   | 5.7           | Collect metrics from performance_schema.events_statements_summary_by_digest summed.   |
| collect.perf_schema.eventswaits                           | 5.5           | Collect metrics from performance_schema.events_waits_summary_global_by_event_name.    |
| collect.perf_schema.file_events                           | 5.6           | Collect metrics from performance_schema.file_summary_by_event_name.                   |
| collect.perf_schema.file_instances                        | 5.5           | Collect metrics from performance_schema.file_summary_by_instance.                     |
| collect.perf_schema.file_instances.remove_prefix          | 5.5           | Remove path prefix in performance_schema.file_summary_by_instance.                    |
| collect.perf_schema.indexiowaits                          | 5.6           | Collect metrics from performance_schema.table_io_waits_summary_by_index_usage.        |
| collect.perf_schema.memory_events                         | 5.7           | Collect metrics from performance_schema.memory_summary_global_by_event_name.          |
| collect.perf_schema.memory_events.remove_prefix           | 5.7           | Remove instrument prefix in performance_schema.memory_summary_global_by_event_name.   |
| collect.perf_schema.tableiowaits                          | 5.6           | Collect metrics from performance_schema.table_io_waits_summary_by_table.              |
| collect.perf_schema.tablelocks                            | 5.6           | Collect metrics from performance_schema.table_lock_waits_summary_by_table.            |
| collect.perf_schema.replication_group_members             | 5.7           | Collect metrics from performance_schema.replication_group_members.                    |
| collect.perf_schema.replication_group_member_stats        | 5.7           | Collect metrics from performance_schema.replication_group_member_stats.               |
| collect.perf_schema.replication_applier_status_by_worker  | 5.7           | Collect metrics from performance_schema.replication_applier_status_by_worker.         |
<br><br>

# optional collector flags

| Argument                        | MySQL Version | Description                                                                                         |
|---------------------------------|---------------|-----------------------------------------------------------------------------------------------------|
| collect.auto_increment.columns  | 5.1           | Collect auto_increment columns and max values from information_schema.                              |
| collect.binlog_size             | 5.1           | Collect the current size of all registered binlog files                                             |
| collect.engine_innodb_status    | 5.1           | Collect from SHOW ENGINE INNODB STATUS.                                                             |
| collect.engine_tokudb_status    | 5.6           | Collect from SHOW ENGINE TOKUDB STATUS.                                                             |
| collect.global_status           | 5.1           | Collect from SHOW GLOBAL STATUS (Enabled by default)                                                |
| collect.global_variables        | 5.1           | Collect from SHOW GLOBAL VARIABLES (Enabled by default)                                             |
| collect.mysql.user              | 5.5           | Collect data from mysql.user table                                                                  |
| collect.slave_status            | 5.1           | Collect from SHOW SLAVE STATUS (Enabled by default)                                                 |
| collect.slave_hosts             | 5.1           | Collect from SHOW SLAVE HOSTS                                                                       |
| collect.heartbeat               | 5.1           | Collect from heartbeat.                                                                             |
| collect.heartbeat.database      | 5.1           | Database from where to collect heartbeat data. (default: heartbeat)                                 |
| collect.heartbeat.table         | 5.1           | Table from where to collect heartbeat data. (default: heartbeat)                                    |
| collect.heartbeat.utc           | 5.1           | Use UTC for timestamps of the current server (pt-heartbeat is called with --utc). (default: false)  |
-->