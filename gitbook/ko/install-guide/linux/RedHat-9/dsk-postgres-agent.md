# dsk-postgres-agent

`postgres agent`는 데이터베이스의 상태를 실시간으로 수집합니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량 등 다양한 정보를 수집할 수 있습니다.\
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

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

# Datasaker 선행 작업을 진행하였나요?
현재 RedHat 9  환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Postgres agent 설치하기
## 1. Postgres 설정 변경
관제하려는 데이터베이스의 `pg_stat_statements` 모듈이 활성화 된 상태인지 확인 부탁드립니다.\
[pg_stat_statements 참조사이트](https://www.postgresql.org/docs/14/pgstatstatements.html)

## 2. Postgres User 권한 설정
`postgres agent`를 설치하기 위해서는 `postgres user`의 권한이 필요합니다.\
`postgres user`의 권한을 확인하고, 권한이 없다면 권한을 부여해주세요.\
필요한 User 권한은 다음과 같습니다.
- `SELECT`
- `UPDATE`
- `DELETE`
- `INSERT`

[postgres user 권한 참조사이트](https://www.postgresql.org/docs/14/sql-grant.html)

## 3. 패키지 설치
```shell
sudo yum install dsk-postgres-agent
```

## 4. Postgres agent 설정
```shell
sudo vi /etc/datasaker/dsk-postgres-agent/agent-config.yml
```
필요에 따라 다음 내용을 수정합니다.

### `agent-config.yml`
```yaml
agent:
  metadata:
    agent_name: dsk-postgres-agent # 에이전트 이름 (별칭) default=dsk-postgres-agent
  option:
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
# 에이전트 이름 (별칭)
[ agent_name: <string> | default = "dsk-postgres-agent" ]

# 관제 대상이 되는 환경이 어떤 클러스터로 묶여있는지에 대한 설정
[ cluster_id: <cluster_id> | default = "unknown" ]
```

#### `option.exporter_config.port`
```yaml
# agent가 사용하는 port number 기존 사용중인 application과 포트 충돌 발생시 임의 값으로 변경
[ port: <uint16> | default = 19187 ]
```

#### `option.exporter_config.args`
```yaml
# 관제하려는 database의 접속권한을 가진 계정 정보와 주소를 입력합니다.
- --data-source-user=<monitoring account name>
- --data-source-pass=<monitoring account pass>
- --data-source-uri=<monitoring database uri> # <ip>:<port>/dbname
```

## 5. 패키지 실행
```shell
sudo systemctl enable dsk-postgres-agent --now
```

## 6. 패키지 실행 상태 확인
```shell
sudo systemctl status dsk-postgres-agent
```

# Postgres agent 제거하기

## 1. 패키지 중단
```shell
sudo systemctl stop dsk-postgres-agent
```

## 2. 패키지 제거
```shell
sudo systemctl stop dsk-postgres-agent &&
sudo yum remove dsk-postgres-agent
```
