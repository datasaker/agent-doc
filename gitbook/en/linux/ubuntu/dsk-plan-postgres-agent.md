# dsk-plan-postgres-agent

`plan-postgres-agent` collects `active sessions` from the database in real time.\
This allows you to gather information about slow queries in your database.\
By detecting slow queries, you can improve database performance by creating indexes, optimizing queries, and more.\
We tailor agent settings to suit your needs to deliver optimal results.\

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

In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor]($%7BPREPARATION\_MANUAL\_KR%7D/)


## Plan Postgres Agent install

### 1. Change Postgres settings

Please check if the `pg_stat_statements` module of the database you want to control is active.\
[pg\_stat\_statements reference site](https://www.postgresql.org/docs/14/pgstatstatements.html)

### 2. Set Postgres User Permissions

In order to install `postgres agent`, permission of `postgres user` is required.\
Check the permission of the `postgres user`, and if not, grant permission.\
Required user rights are as follows.

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[postgres user permission reference site](https://www.postgresql.org/docs/14/sql-grant.html)

### 3. Install the package

```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-plan-postgres-agent
```

### 4. Register Postgres agent settings

#### Required fields

Required input items are as follows. Enter the values ​​according to your Postgres settings.

| Entities | Description |
| --------------------------------- | -------------------------- |
| agent.data\_source\_name.user | Enter the Postgres user ID. |
| agent.data\_source\_name.password | Enter the Postgres user password. |
| agent.data\_source\_name.address | Enter the Postgres address. |
| agent.data\_source\_name.port | Enter the Postgres port. |
| agent.data\_source\_name.DBName | Enter the Postgres database. |

#### Enter options

```shell
vi /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml
```

Modify the following as needed.

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent" # agent name (alias) default=dsk-plan-postgres-agent
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

### 5. Run the package

```shell
systemctl enable dsk-plan-postgres-agent --now
```

### 6. Check package execution status

```shell
systemctl status dsk-plan-postgres-agent
```

or

```shell
serivce dsk-plan-postgres-agent
```

\
\


## Remove Plan Postgres Agent

### 1. Abort the package

```shell
systemctl stop dsk-plan-postgres-agent
```

### 2. Remove packages

```shell
sudo apt remove dsk-plan-postgres-agent
```