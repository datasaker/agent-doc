# dsk-postgres-agent

The `postgres agent` collects the state of the database in real time.\
This allows you to collect various information such as database performance indicators and resource usage.\
Based on the collected data, it is possible to identify and respond to performance bottlenecks in the database.\
We tailor agent settings to your needs to deliver optimal results.

# Supported version
|version|support|
|---|---|
|postgres 15|X|
|postgres 14|O|
|postgres 13|X|
|postgres 12|X|
|postgres 11|X|
|postgres 10|X|
|postgres 9|X|
|postgres 8|X|

# Did you run the Datasaker predecessor?
If the preceding task of `DataSaker` has not been carried out in the current RedHat 7 environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](${PREPARATION_MANUAL_KR})

# Install the Postgres agent
## 1. Change Postgres Settings
Please check if the `pg_stat_statements` module of the database you want to monitor is active.\
[pg_stat_statements reference site](https://www.postgresql.org/docs/14/pgstatstatements.html)

## 2. Set Postgres User Permissions
In order to install `postgres agent`, permission of `postgres user` is required.\
Check the permission of the `postgres user`, and if not, grant permission.\
Required user rights are as follows.
-`SELECT`
-`UPDATE`
-`DELETE`
-`INSERT`

[postgres user permission reference site](https://www.postgresql.org/docs/14/sql-grant.html)

## 3. Install the package
```shell
sudo yum install dsk-postgres-agent
```

## 4. Postgres agent configuration
```shell
sudo vi /etc/datasaker/dsk-postgres-agent/agent-config.yml
```
Modify the following as needed.

### `agent-config.yml`
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
        - --data-source-user= # <monitoring account name>
        - --data-source-pass= # <monitoring account pass>
        - --data-source-uri= # <monitoring database uri> # <ip>:<port>/dbname
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-postgres-agent
        url: localhost:19187
        filtering_configs:
          rule: drop
```

#### `metadata`
```yaml
# agent name (alias)
[ agent_name: <string> | default = "dsk-postgres-agent" ]

# Settings for which clusters the environment to be controlled are grouped into
[ cluster_id: <cluster_id> | default = "unknown" ]
```

#### `option.exporter_config.port`
```yaml
# The port number used by the agent is changed to a random value when a port conflict occurs with an existing application.
[ port: <uint16> | default = 19187 ]
```

#### `option.exporter_config.args`
```yaml
# Enter the account information and address that has access to the database you want to control.
- --data-source-user=<monitoring account name>
- --data-source-pass=<monitoring account pass>
- --data-source-uri=<monitoring database uri> # <ip>:<port>/dbname
```

## 5. Run the package
```shell
sudo systemctl enable dsk-postgres-agent --now
```

## 6. Check package execution status
```shell
sudo systemctl status dsk-postgres-agent
```

# Uninstall the Postgres agent

## 1. Package abort
```shell
sudo systemctl stop dsk-postgres-agent
```

## 2. Remove packages
```shell
sudo systemctl stop dsk-postgres-agent &&
sudo yum remove dsk-postgres-agent
```