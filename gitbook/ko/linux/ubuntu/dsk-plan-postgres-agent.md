# dsk-plan-postgres-agent

`plan-postgres-agent`는 데이터베이스의 `active session`을 실시간으로 수집합니다.\
이를 통해 데이터베이스의 슬로우 쿼리에 대한 정보를 수집할 수 있습니다.\
슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.\

## Supported version

| version     | support |
| ----------- | ------- |
| postgres 15 | X       |
| postgres 14 | O       |
| postgres 13 | X       |
| postgres 12 | X       |
| postgres 11 | X       |
| postgres 10 | X       |
| postgres 9  | X       |
| postgres 8  | X       |

## DataSaker 선행 작업을 진행하였나요?

현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업]($%7BPREPARATION\_MANUAL\_KR%7D/)


## Plan Postgres Agent install

### 1. Postgres 설정 변경

관제하려는 데이터베이스 `pg_stat_statements` 모듈의 활성화 된 상태인지 확인 부탁드립니다.\
[pg\_stat\_statements 참조사이트](https://www.postgresql.org/docs/14/pgstatstatements.html)

### 2. Postgres User 권한 설정

`postgres agent`를 설치하기 위해서는 `postgres user`의 권한이 필요합니다.\
`postgres user`의 권한을 확인하고, 권한이 없다면 권한을 부여해주세요.\
필요한 User 권한은 다음과 같습니다.

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[postgres user 권한 참조사이트](https://www.postgresql.org/docs/14/sql-grant.html)

### 3. 패키지 설치

```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-plan-postgres-agent
```

### 4. Postgres agent 설정값 등록

#### 필수입력 항목

필수입력 항목은 다음과 같습니다. Postgres 설정에 맞게 값을 넣어주세요

| Entity                            | Description                |
| --------------------------------- | -------------------------- |
| agent.data\_source\_name.user     | Postgres user 아이디를 입력합니다.  |
| agent.data\_source\_name.password | Postgres user 패스워드를 입력합니다. |
| agent.data\_source\_name.address  | Postgres 주소를 입력합니다.        |
| agent.data\_source\_name.port     | Postgres 포트를 입력합니다.        |
| agent.data\_source\_name.DBName   | Postgres 데이터베이스를 입력합니다.    |

#### 옵션입력

```shell
vi /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml
```

필요에 따라 다음 내용을 수정합니다.

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent" # 에이전트 이름 (별칭) default=dsk-plan-postgres-agent
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

### 5. 패키지 실행

```shell
systemctl enable dsk-plan-postgres-agent --now
```

### 6. 패키지 실행 상태 확인

```shell
systemctl status dsk-plan-postgres-agent
```

또는

```shell
serivce dsk-plan-postgres-agent
```

\
\


## Plan Postgres Agent 제거하기

### 1. 패키지 중단

```shell
systemctl stop dsk-plan-postgres-agent
```

### 2. 패키지 제거

```shell
sudo apt remove dsk-plan-postgres-agent
```
