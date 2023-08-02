# dsk-postgres-agent

The `postgres agent` collects the state of the database in real time.\
This allows you to collect various information such as database performance indicators and resource usage.\
Based on the collected data, it is possible to identify and respond to performance bottlenecks in the database.\
We tailor agent settings to your needs to deliver optimal results.

## Supported version

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

## Have you done the Datasaker predecessor?

If the preceding task of `DataSaker` has not been performed in the current environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](README.md)

## Install the Postgres agent

### 1. Change Postgres settings

Please check if the `pg_stat_statements` module of the database you want to monitor is active.\
[pg_stat_statements reference site](https://www.postgresql.org/docs/14/pgstatstatements.html)

### 2. Set Postgres User Permissions

In order to install `postgres agent`, permission of `postgres user` is required.\
Check the permission of the `postgres user`, and if not, grant permission.\
Required user rights are as follows.

-`SELECT`
-`UPDATE`
-`DELETE`
-`INSERT`

[postgres user permission reference site](https://www.postgresql.org/docs/14/sql-grant.html)

### 3. Install the package
```shell
yum install dsk-postgres-agent
```
### 4. Postgres agent configuration
```shell
vi /etc/datasaker/dsk-postgres-agent/agent-config.yml
```
Modify the following as needed.

#### `agent-config.yml`
```yaml
agent:
  metadata:
    agent_name: dsk-postgres-agent
  option:
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
##### `metadata`
```yaml
# 에이전트 이름 (별칭)
[ agent_name: <string> | default = "dsk-postgres-agent" ]

# 관제 대상이 되는 환경이 어떤 클러스터로 묶여있는지에 대한 설정
[ cluster_id: <cluster_id> | default = "unknown" ]
```
A description of each setting follows.

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :----------: | :------------: |
| `agent_name` | Agent Name (Alias) | dsk-postgres-agent | N/A |
| `cluster_id` | Settings for which clusters the environment to be controlled are grouped into | unknown | N/A |

##### `option.exporter_config.port`
```yaml
[ port: <uint16> | default = 19187 ]
```
A description of each setting follows.

| **Settings** | **Description** | **Default** | **Required** |
| ------------ | ---------------------------------------------------------------------------------------------------- | :----------: | :------------: |
| `port` | The port number used by the agent is changed to a random value when a port conflict occurs with an existing application | 19187 | N/A |

##### `option.exporter_config.args`
```yaml
# 관제하려는 database의 접속권한을 가진 계정 정보와 주소를 입력합니다.
- --data-source-user=<monitoring account name>
- --data-source-pass=<monitoring account pass>
- --data-source-uri=<monitoring database uri> # <ip>:<port>/dbname
```
The explanation of each argument is as follows.

| **Settings** | **Description** | **Default** | **Required** |
| ------------ | ---------------------------------------------------------------------------------------------------- | :----------: | :------------: |
| `--data-source-user` | Enter the account information that has access to the database you want to control. | N/A | **✓** |
| `--data-source-pass` | Enter the password of the account that has access to the database you want to control. | N/A | **✓** |
| `--data-source-uri` | Enter the address of the database you want to monitor. | N/A | **✓** |

### 5. Run the package
```shell
systemctl enable dsk-postgres-agent --now
```
### 6. Check package execution status
```shell
systemctl status dsk-postgres-agent
```
## Remove the Postgres agent

### 1. Abort the package
```shell
systemctl stop dsk-postgres-agent
```
### 2. Remove packages
```shell
yum remove dsk-postgres-agent
```
