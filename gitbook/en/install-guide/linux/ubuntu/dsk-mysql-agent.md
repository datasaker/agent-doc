#Coming Soon
<!--
# Installing DataSaker Mysql agent in Ubuntu environment (Beta)
'Mysql agent' collects database status and slow queries in real time.
This allows you to collect a variety of information, including performance metrics, resource usage, and slow queries from your database.
Based on the collected data, you can identify and respond to database performance bottlenecks.
It can also detect slow queries to improve database performance by creating indexes, optimizing queries, and more.
We tailor agent settings to your needs to deliver optimal results.
<br><br>

# Supported version
|version|support|
|---|---|
|MySQL 8.0.33|O|

<br><br>

# Did you run the DataSaker predecessor?
In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](${PREPARATION_MANUAL_KR})
<br><br>

# Install Mysql agent
## 1. Change MySQL settings
Please check if the `performance_schema=ON` module of the database you want to control is activated.\
[performance_schema reference site](https://dev.mysql.com/doc/refman/8.0/en/performance-schema-quick-start.html)

## 2. Setting MySQL User Privileges
In order to install `MySQL agent`, you need to grant permission to `MySQL User`.\
Check the privileges of `MySQL user`, and grant privileges if they do not exist.\
Required user rights are as follows.
-`SELECT`
-`UPDATE`
-`DELETE`
-`INSERT`

[MySQL user permission reference site](https://dev.mysql.com/doc/refman/8.0/en/grant.html)

## 3. Install the package
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mysql-agent
```

## 4. Register MySQL agent settings
### Register environment variables
```
env DATA_SOURCE_NAME=ID:Password@MySQLIP:Port
```

### Option input
Write the contents to `/etc/datasaker/dsk-mysql-agent/agent-config.yml`.
```yaml
agent:
  metadata:
    agent_name: 'dsk-mysql-agent' # agent name (alias) default=dsk-node-agent
  options:
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
        url: localhost:19104 #
        filtering_configs:
          rule: drop
```


## 5. Run the package
```bash
$ sudo -E dsk-mysql-agent start
Agent is running
```

## 6. Check package execution status
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

# Uninstall Mysql agent
## 1. Package abort
```bash
sudo dsk-mysql-agent stop
```

## 2. Remove packages
```bash
sudo apt remove dsk-mysql-agent
```
-->