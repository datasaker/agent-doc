# Coming Soon
<!--
# Installing MySQL Agent on Kubernetes with helm

## DId you install Base Agent?

If you haven't installed **Base Agent**, please install **Base Agent** first. (Link)

## Edit the values.yaml file from ~/datasaker/agent-helm/values.yaml

Remove the square brackets after 'list:' in the 'list:' line.
Remove comments beside '-name;' and enter the information of MySQL

```shell
vim ~/datasaker/agent-helm/values.yaml
```

Edit mysqlAgent

```yaml
mysqlAgent:
  tolerations: []
  list: #[] <- Remove the square brackets after 'list:'. or comment out the brackets.
    - name: 'mysql-1'
      targetIp: '127.0.0.1'
      targetPort: '3306'
      user: 'user'
      pass: "pass"
      imgPolicy: 'Always'
      imgVersion: 'develop'
      logLevel: 'DEBUG'
      extraArgs:
        - --collect.heartbeat
        - --collect.heartbeat.utc
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
```

### Strategy of Agent Deployment

Define agent deployment settings to `toleration` section in '~/datasaker/agent-helm/values.yaml' file. For a detailed explanation of toleration, check [Official Kubernetes Document Link](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/).

## helm upgrade

```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

## extraArgs value

List of extra arguments that can be passed to the **DataSaker MySQL Agent**.

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

## optional collector flags

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