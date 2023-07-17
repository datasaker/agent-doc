# dsk-postgres-agent

## Installing DataSaker Postgres agents in Kubernetes environment

`Postgres agents` consists of `postgres-agent` and `plan-postgres-agent`.\
This allows you to collect a variety of information, such as performance metrics, resource usage, and slow queries from your database.\
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

Postgres agent consists of `postgres agent` and `plan-postgres agent`.

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

If the preceding task of `DataSaker` has not been carried out in the current Kubernetes environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](dsk-postgres-agent/en/$%7BPREPARATION\_MANUAL\_KR%7D/)


## Install the Postgres agent

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

### 3. Register Postgres agent settings

#### Required fields

Required input items are as follows. Enter the values ​​according to your Postgres settings.

| Entities | Description |
| --------------------------------- | -------------------------------- |
| postgresAgents.list\[].name | Postgres Gives a distinguishing name. |
| postgresAgents.list\[].targetAddr | Enter the Postgres target address. |
| postgresAgents.list\[].targetPort | Enter Postgres as the target port. |
| postgresAgents.list\[].database | Enter the Postgres database name. |
| postgresAgents.list\[].user | Enter the Postgres user ID. |
| postgresAgents.list\[].pass | Enter the Postgres user password. |

#### Enter options

```shell
cat << EOF >> ~/datasaker/config.yaml

postgresAgents:
  list:
    - name: 'my-postgres'
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19187
      tolerances: []
      targetAddr: '127.0.0.1'
      targetPort: '5432'
      database: "database"
      user: 'user'
      pass: "pass"
      exporterArgs: []
      postgresPlan: true
      explain:
        scrape_interval: 5s
        scrape_timeout: 5s
        slow_query_standard: 5s
        executor_number: 10
        sender_number: 10
      resources:
        requests:
          CPU: 100m
          Memory: 512 Mi
        limits:
          CPU: 1000m
          Memory: 1000 Mi
EOF
```

### 4. Activate the Postgres agent

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```