# 쿠버네티스 환경에서 DataSaker Postgres agents 설치하기
`Postgres agents`는 `postgres-agent`와 `plan-postgres-agent`로 구성되어 있습니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.\
고객의 요구사항에 따라 `plan-postgres-agent`는 `on/off` 형태로 사용 할 수 있습니다.
<br><br>

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

<br><br>

# Agent 구성
Postgres agent는 `postgres agent`와 `plan-postgres agent`로 구성되어 있습니다.
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
<br><br>

# DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})
<br><br>

# Postgres agent 설치하기
## 1. Postgres 설정 변경
관제하려는 데이터베이스 `pg_stat_statements` 모듈의 활성화 된 상태인지 확인 부탁드립니다.\
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

## 3. Postgres agent 설정값 등록
### 필수입력 항목
필수입력 항목은 다음과 같습니다. Postgres 설정에 맞게 값을 넣어주세요
|Entity|Description|
|---|---|
|postgresAgents.list[].name|Postgres 구분할 수 있는 명칭을 부여합니다.|
|postgresAgents.list[].targetAddr|Postgres를 target address를 입력합니다.|
|postgresAgents.list[].targetPort|Postgres를 target port를 입력합니다.|
|postgresAgents.list[].database|Postgres Database명을 입력합니다.|
|postgresAgents.list[].user|Postgres user 아이디를 입력합니다.|
|postgresAgents.list[].pass|Postgres user 패스워드를 입력합니다.|

### 옵션입력
```shell
cat << EOF >> ~/datasaker/config.yaml

postgresAgents:
  list:
    - name: 'my-postgres'
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19187
      tolerations: []
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
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
```

## 4. Postgres agent 활성화
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
