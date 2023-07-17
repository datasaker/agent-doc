# dsk-postgres-agent

The `postgres agent` collects the state of the database in real time.\
This allows you to collect various information such as database performance indicators and resource usage.\
Based on the collected data, it is possible to identify and respond to performance bottlenecks in the database.\
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

In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor]($%7BPREPARATION\_MANUAL\_KR%7D/)


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

### 3. Install the package

```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres-agent
```

### 4. agent-config settings

Write the contents to `/etc/datasaker/dsk-postgres-agent/agent-config.yml`.

```yaml
agent:
  metadata:
    agent_name: dsk-postgres-agent # Agent name (alias) default=dsk-postgres-agent
  options:
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

### 5. Run the package

```shell
systemctl enable dsk-postgres-agent --now
```

### 6. Check package execution status

```shell
systemctl status dsk-postgres-agent
```

or

```shell
service dsk-postgres-agent
```

## Remove the Postgres agent

### 1. Abort the package

```shell
systemctl stop dsk-postgres-agent
```

### 2. Remove packages

```shell
sudo apt remove dsk-postgres-agent
```