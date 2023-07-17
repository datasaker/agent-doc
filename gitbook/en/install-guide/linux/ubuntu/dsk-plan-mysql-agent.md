# dsk-plan-mysql-agent

'Mysql agent' collects database status and slow queries in real time. This allows you to collect a variety of information, including performance metrics, resource usage, and slow queries from your database. Based on the collected data, you can identify and respond to database performance bottlenecks. It can also detect slow queries to improve database performance by creating indexes, optimizing queries, and more. We tailor agent settings to your needs to deliver optimal results.

## Supported version

| version | support |
| ------------ | ------- |
| MySQL 8.0.33 | O |

## Did you run the DataSaker predecessor?

In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor]($%7BPREPARATION\_MANUAL\_KR%7D/)

## Plan MySQL Agent install

### 1. Change MySQL settings

Please check if the `performance_schema=ON` module of the database you want to control is activated.\
[performance\_schema reference site](https://dev.mysql.com/doc/refman/8.0/en/performance-schema-quick-start.html)

### 2. MySQL User Privilege Settings

In order to install `MySQL agent`, you need to grant permission to `MySQL User`.\
Check the privileges of `MySQL user`, and grant privileges if they do not exist.\
Required user rights are as follows.

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[MySQL user permission reference site](https://dev.mysql.com/doc/refman/8.0/en/grant.html)

### 3. Install the package

```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-plan-mysql-agent
```

### 4. Register MySQL agent settings

#### Required fields

Required input items are as follows. Enter the values ​​according to your MySQL settings.

| Entities | Description |
| --------------------------------- | ----------------------- |
| agent.data\_source\_name.user | Enter the MySQL User ID. |
| agent.data\_source\_name.password | Enter the MySQL User password. |
| agent.data\_source\_name.address | Enter the MySQL address. |
| agent.data\_source\_name.port | Enter the MySQL port. |
| agent.data\_source\_name.DBName | Enter the MySQL database name. |

#### Enter options

```shell
vi /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml
```

Modify the following as needed.

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-mysql-agent" # agent name (alias) default=dsk-plan-mysql-agent
  data_source_name:
    user: 'user'
    password: 'pass'
    address: '127.0.0.1'
    port: '3306'
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

```bash
sudo dsk-plan-mysql-agent start
```

### 6. Check package execution status

#### Running

```bash
systemctl status dsk-plan-mysql-agent
```

or

```shell
serivce dsk-plan-mysql-agent
```

\
\


## Remove Log agent

### 1. Abort the package

```bash
sudo dsk-plan-mysql-agent stop
```

### 2. Remove packages

```bash
sudo apt remove dsk-plan-mysql-agent
```