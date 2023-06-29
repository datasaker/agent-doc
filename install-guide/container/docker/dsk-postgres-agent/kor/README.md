# 도커 환경에 DataSaker PostgreSQL agent 설치하기

`Postgres agent`는 `postgres-agent`와 `plan-postgres-agent`로 구성되어 있습니다. \
이를 통해 Postgresql의 성능 지표,리리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.\
고객의 요구사항에 따라 `plan-postgres-agent`는 `on/off` 형태로 사용 할 수 있습니다.

# Supported version
|version|support|
|---|---|
|postgres 14|O|



# Agent 구성

Postgres agent는 `postgres agent`와 `plan-postgres-agent`로 구성되어 있습니다.

## postgres agent

`postgres agent`는 데이터베이스의 상태를 실시간으로 수집합니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량 등 다양한 정보를 수집할 수 있습니다.\
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## plan-postgres agent

`plan-postgres-agent`는 데이터베이스의 `active session`을 실시간으로 수집합니다.\
이를 통해 데이터베이스의 슬로우 쿼리에 대한 정보를 수집할 수 있습니다.\
슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Postgres agent 설치하기

## 1. Postgres agent 설정 변경
### 필수 설정
| Parameter | Value | Description |
|-----------|-------|-------------|
| shared_preload_libraries | pg_stat_statements | `pg_stat_statements` extension을 이용하여 `pg_dsk_sql_stat.*` metric을 추출합니다.|
| track_activity_query_size | 4096 | `pg_stat_activity`, `pg_stat_statements`의 SQL 텍스트 길이를 증가시킵니다.|
### 선택 설정
| Parameter | Value | Description |
|-----------|-------|-------------|
| pg_stat_statements.track | ALL | `stored producers`, `function` 내에서의 구문을 추적합니다.|
| pg_stat_statements.max | 10000 | `pg_stat_statements`의 추적 쿼리를 증가시킵니다. 이 설정은 다양한 클라이언트, 다양한 유형의 대용량 데이터베이스에 권장됩니다.|
| pg_stat_statements.track_utility | 0 | `PREPARE`, `EXPLAIN`같은 유틸리 명령에 대한 추적을 비활성화 합니다. |

관제하려는 데이터베이스의 `pg_stat_statements` 모듈이 활성화 된 상태인지 확인 부탁드립니다.
```
show shared_preload_libraries;


 shared_preload_libraries
--------------------------
 pg_stat_statements
(1 row)
```
[pg_stat_statements 참조사이트](https://www.postgresql.org/docs/14/pgstatstatements.html)


## 2. Postgres User 권한 설정
`postgres agent`를 사용하기 위해서는 `pg_monitor`의 권한이 필요합니다.\
`datasaker` 전용계정을 생성하세요, 전용계정을 생성할때는 `superuser`를 사용해야 합니다.
### datasaker 전용 계정
```sql
CREATE USER datasaker WITH password '<PASSWORD>';
```

### datasaker 전용 데이터베이스 생성
```sql
CREATE DATABASE datasaker;
GRANT USAGE ON SCHEMA public TO datasaker;
GRANT pg_monitor TO datasaker;
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
```
[postgres user 권한 참조사이트](https://www.postgresql.org/docs/14/sql-grant.html)

## 3. Postgres agent 설정값 등록

### 필수입력 사항 안내

에이전트를 연결하기 위해서는 수집하고자 하는 PostgreSQL 서버의 주소, 데이터베이스, 유저 ID와 패스워드를 에이전트에 설정해야 합니다.

```shell
 DSK_PG_USER=datasaker
 DSK_PG_SOURCE_PASS=<password>
 DSK_PG_DB_NAME=datasaker
 DSK_PG_HOST=<host>
 DSK_PG_PORT=<port>
```

예를 들어, 주소가 `192.168.123.132`이고, 기본 포트 `5432`에 서비스중인 PostgreSQL를 수집하기 위해서는 터미널에 다음과 같이 설정할 수 있습니다.

```shell
 DSK_PG_USER=datasaker
 DSK_PG_SOURCE_PASS=postgres
 DSK_PG_DB_NAME=datasaker
 DSK_PG_HOST=192.168.123.132
 DSK_PG_PORT=5432
```

### Postgres agent 설정값 등록

이제 터미널에 다음 명령어를 입력하여 dsk-postgres-agent와 dsk-plan-postgres-agent의 설정파일을 생성합니다.

```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/postgres-config.yml
agent:
  metadata:
    agent_name: dsk-postgres-agent
  option:
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

| 설정값                         | 설명                       |
|-----------------------------|--------------------------|
| data_source_name.user       | DB 유저 명                  |
| data_source_name.password   | DB 유저 비밀번호              |
| data_source_name.address    | PostgreSQL 서버 접속 주소      |
| data_source_name.port       | PostgreSQL 서버 접속 포트      |
| data_source_name.DBName     | PostgreSQL 데이터베이스        |
| explain.scrape_interval     | activity session 수집하는 주기 |
| explain.scrape_timeout      | 수집 타임아웃                  |
| explain.slow_query_standard | 슬로우 쿼리 기준 시간             |

### Postgres agent 설치

1. 데이터세이커가 사용할 로컬 디렉터리 생성합니다.

```shell
 sudo mkdir -p /var/datasaker
 sudo chown -R datasaker:datasaker /var/datasaker/ 
```

2. 도커 명령어를 서버에 입력합니다.

```shell
 DSK_PG_URI=${DSK_PG_HOST}:${DSK_PG_PORT}/${DSK_PG_DB_NAME}?sslmode=disable
 docker run -d --name dsk-postgres-agent\
  -v /var/datasaker/:/var/datasaker/\
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
  -v ~/.datasaker/postgres-config.yml:/etc/datasaker/dsk-postgres-agent/agent-config.yml:ro\
  -e DKS_LOG_LEVEL=info\
  -e DATA_SOURCE_USER=${DSK_PG_USER}\
  -e DATA_SOURCE_PASS=${DSK_PG_SOURCE_PASS}\
  -e DATA_SOURCE_URI=${DSK_PG_URI}\
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
