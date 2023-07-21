# dsk-plan-postgres-agent

## Installing Datasaker's Plan-Postgres agent on Amazon Linux 2023 environment (Beta)

`plan-postgres-agent` collects `active sessions` from the database in real time.\
This allows you to gather information about slow queries in your database.\
By detecting slow queries, you can improve database performance by creating indexes, optimizing queries, and more.\
We tailor agent settings to your needs to deliver optimal results.

## Supported version

| version | support |
| ----------- | ------- |
| postgres 15 | X |
| postgres 14 | O |
| postgres 13 | X |
| postgres 12 | X |
| postgres 11 | X |
| postgres 10 | X |
| postgres 9 | X |
| postgres 8 | X |

## Did you run the DataSaker predecessor?

In the current Amazon Linux 2023 environment, if the preceding task of 'DataSaker' has not been carried out, please proceed with the preceding task of 'DataSaker' first. [DataSaker predecessor](dsk-plan-postgres-agent/kor/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Plan Postgres Agent Install

### 1. Set Postgres User Permissions

In order to install `plan-postgres-agent`, permission of `postgres user` is required.\
Check the permission of the `postgres user`, and if not, grant permission.\
Required user rights are as follows.

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[postgres user permission reference site](https://www.postgresql.org/docs/14/sql-grant.html)

### 2. Install the package

```shell
yum install dsk-plan-postgres-agent
```

### 3. Plan Postgres Agent Settings

```shell
vi /etc/datasaker/dsk-plan-postgres-agent/agent-config.yml
```

Modify the following as needed.

#### `agent-config.yml`

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent" # agent name (alias) default=dsk-plan-postgres-agent
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

**`metadata`**

```yaml
# agent name (alias)
[ agent_name: <string> | default = "dsk-trace-agent" ]

# Settings for which clusters the environment to be controlled are grouped into
[ cluster_id: <cluster_id> | default = "unknown" ]
```

**`data_source_name`**

```yaml
# postgres account name
[ user: <string> | required]
# postgres account password
[ password: <string> | required]
# postgres server url
[ address: <string> | required]
# postgres server port
[ port: <uint16> | required]
# postgres server database name
[ DBName: <string> | required]
```

**`explain`**

```yaml
# activity session scrape cycle
[scrape_interval: <seconds> | default=30s]
# timeout time of activity session scrape query
[scrape_timeout: <seconds> | default=5s]
# based on slow query
[slow_query_standard: <seconds> | default=5s ]
# number of threads to run explain
[executor_number: <int8> | default=10 ]
# Number of threads sending explain results
[sender_number: <int8> | default=10 ]
# activity query buffer
[activity_query_buffer: <int16> | default=50 ]
# explain result buffer
[plan_sender_buffer: <int16> | default=50 ]
```

### 4. Run the package

```shell
systemctl enable dsk-plan-postgres-agent --now
```

### 5. Check package execution status

```shell
systemctl status dsk-plan-postgres-agent
```

## Remove Plan Postgres Agent

### 1. Abort the package

```shell
systemctl stop dsk-plan-postgres-agent
```

### 2. Remove packages

```shell
yum remove dsk-plan-postgres-agent
```