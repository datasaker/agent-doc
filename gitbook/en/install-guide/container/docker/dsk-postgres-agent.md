# dsk-postgres-agent

## Install DataSaker PostgreSQL agent in docker environment

`Postgres agent` consists of `postgres-agent` and `plan-postgres-agent`.\
This allows you to collect various information such as Postgresql's performance metrics, resource usage, slow queries, etc.\
Depending on the customer's requirements, `plan-postgres-agent` can be used in the form of `on/off`.

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

## Configure Agent

The Postgres agent consists of `postgres agent` and `plan-postgres-agent`.

### postgres agent

The `postgres agent` collects the state of the database in real time.\
This allows you to collect various information such as database performance indicators and resource usage.\
Based on the collected data, it is possible to identify and respond to performance bottlenecks in the database.\
We tailor agent settings to your needs to deliver optimal results.

### plan-postgres agent

`plan-postgres-agent` collects `active sessions` from the database in real time.\
This allows you to gather information about slow queries in your database.\
By detecting slow queries, you can improve database performance by creating indexes, optimizing queries, and more.\
We tailor agent settings to your needs to deliver optimal results.

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Docker environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](dsk-postgres-agent/en/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Install the Postgres agent

### 1. Change Postgres agent settings

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

### 3. Register Postgres agent settings

#### Required input information

To connect the agent, the address of the PostgreSQL server to be collected, database, user ID and password must be set on the agent.

```shell
 DSK_PG_USER=<user>
 DSK_PG_SOURCE_PASS=<password>
 DSK_PG_DB_NAME=<database>
 DSK_PG_HOST=<host>
 DSK_PG_PORT=<port>
```

For example, to collect PostgreSQL serviced at the address `192.168.123.132` and the default port `5432`, the terminal can be set as follows.

```shell
 DSK_PG_USER=postgres
 DSK_PG_SOURCE_PASS=postgres
 DSK_PG_DB_NAME=postgres
 DSK_PG_HOST=192.168.123.132
 DSK_PG_PORT=5432
```

#### Register Postgres agent settings

Now, enter the following command in the terminal to create configuration files for dsk-postgres-agent and dsk-plan-postgres-agent.

```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/postgres-config.yml
agent:
  metadata:
    agent_name: dsk-postgres-agent
  options:
    exporter_config:
      command: /etc/datasaker/target-exporter
      port: 9187
    scrape_configs:
      - job_name: dsk-postgres-agent
        metrics_path: /metrics/short
        url: localhost:9187
      - job_name: dsk-postgres-agent-long
        scrape_interval: 60s
        scrape_timeout: 10s
        metrics_path: /metrics/long
        url: localhost:9187
        filtering_configs:
          rule: drop
EOF

cat << EOF > ~/.datasaker/plan-postgres-config.yml
agent:
  metadata:
    agent_name: dsk-plan-postgres-agent
  data_source_name:
    user: ${DSK_PG_USER}
    password: ${DSK_PG_SOURCE_PASS}
    address: ${DSK_PG_HOST}
    port: ${DSK_PG_PORT}
    DBName: ${DSK_PG_DB_NAME}
  explain:
    scrape_interval: 5s
    scrape_timeout: 5s
    slow_query_standard: 1s
EOF
```

| set value | Description |
| ----------------------------- | ------------------------ |
| data\_source\_name.user | DB user name |
| data\_source\_name.password | DB user password |
| data\_source\_name.address | PostgreSQL server connection address |
| data\_source\_name.port | PostgreSQL server connection port |
| data\_source\_name.DBName | PostgreSQL database |
| explain.scrape\_interval | Activity session collection cycle |
| explain.scrape\_timeout | collection timeout |
| explain.slow\_query\_standard | Slow Query Base Time |

#### Install the Postgres agent

1. Create a local directory to be used by Data Shaker.

```shell
 sudo mkdir -p /var/datasaker
 sudo chown -R datasaker:datasaker /var/datasaker/
```

2. Enter the docker command into the server.

```shell
 DSK_PG_URI=${DSK_PG_HOST}:${DSK_PG_PORT}/${DSK_PG_DB_NAME}?sslmode=disable
 docker run -d --name dsk-postgres-agent\
  -v /var/datasaker/:/var/datasaker/\
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
  -v ~/.datasaker/postgres-config.yml:/etc/datasaker/dsk-postgres-agent/agent-config.yml:ro\
  -e DKS_LOG_LEVEL=info\
  -e DATA_SOURCE_USER=${DSK_PG_USER}\
  -e DATA_SOURCE_PASS=${DSK_PG_SOURCE_PASS}\-e DATA_SOURCE_URI=${DSK_PG_URI}\
  --restart=always\
  datasaker/dsk-postgres-agent
  docker run -d --name dsk-plan-postgres-agent\
    -v /var/datasaker/:/var/datasaker/\
    -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
    -v ~/.datasaker/plan-postgres-config.yml:/etc/datasaker/dsk-plan-postgres-agent/agent-config.yml:ro\
    -e DKS_LOG_LEVEL=info\
    --restart=always\
    datasaker/dsk-plan-postgres-agent
```