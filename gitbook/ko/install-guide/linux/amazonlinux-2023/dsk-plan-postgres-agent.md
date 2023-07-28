# dsk-plan-postgres-agent

`plan-postgres-agent`는 데이터베이스의 `active session`을 실시간으로 수집합니다.\
이를 통해 데이터베이스의 슬로우 쿼리에 대한 정보를 수집할 수 있습니다.\
슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

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

현재 환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업]($%7BPREPARATION\_MANUAL\_KR%7D/)

## Plan Postgres Agent Install

### 1. Postgres User 권한 설정
`plan-postgres agent`를 사용하기 위해서는 `pg_monitor`의 권한이 필요합니다.\
`datasaker` 전용계정을 생성하세요, 전용계정을 생성할때는 `superuser`를 사용해야 합니다.
#### datasaker 전용 계정
```sql
CREATE USER datasaker WITH password '<PASSWORD>';
```

#### datasaker 전용 데이터베이스 생성
```sql
CREATE DATABASE datasaker;
GRANT USAGE ON SCHEMA public TO datasaker;
GRANT pg_monitor TO datasaker;
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
```
[postgres user 권한 참조사이트](https://www.postgresql.org/docs/14/sql-grant.html)

### 2. 패키지 설치

```shell
yum install dsk-plan-postgres-agent
```

### 3. Plan Postgres Agent 설정

```shell
vi /etc/datasaker/dsk-plan-postgres-agent/agent-config.yml
```

필요에 따라 다음 내용을 수정합니다.

#### `agent-config.yml`

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres-agent" # <agent_alias_name> default=dsk-plan-postgres-agent
  data_source_name:
    user: datasaker
    password: # <user_password>
    address: # <database_address>
    port: # <database_port>
    DBName: # <database_name>
  explain:
    scrape_interval: 30s # <activity_session_scrape_time>
    scrape_timeout: 5s # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s # <slow_query_standard> 
    executor_number: 10 # <explain executor number>
    sender_number: 10 # <explain sender number>
    activity_query_buffer: 50 # <activity query buffer>
    plan_sender_buffer: 50 # <explain result buffer>
```

각 설정에 대한 설명은 다음과 같습니다.

| **Settings**               | **Description**                                                                                     | **Default** | **Required** |
| -------------------------- | --------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent.metadata.agent_name` | 에이전트 이름 (별칭)                                                                                   |     dsk-plan-postgres-agent     |     **✓**    |
| `agent.data_source_name.user` | postgres 계정 명                                                                                      |     N/A     |     **✓**    |
| `agent.data_source_name.password` | postgres 계정 암호                                                                                    |     N/A     |     **✓**    |
| `agent.data_source_name.address` | postgres 서버 url                                                                                     |     N/A     |     **✓**    |
| `agent.data_source_name.port` | postgres 서버 port                                                                                    |     N/A     |     **✓**    |
| `agent.data_source_name.DBName` | postgres 서버 데이터베이스 명                                                                            |     N/A     |     **✓**    |
| `agent.explain.scrape_interval` | activity session scrape 주기                                                                         |    30s      |              |
| `agent.explain.scrape_timeout` | activity session scrape query의 timeout 시간                                                           |     5s      |              |
| `agent.explain.slow_query_standard` | slow query 기준                                                                                      |     5s      |              |
| `agent.explain.executor_number` | explain을 실행하기 위한 thread 수                                                                    |     10      |              |
| `agent.explain.sender_number` | explain 결과를 전송하는 thread 수                                                                     |     10      |              |
| `agent.explain.activity_query_buffer` | activity query buffer                                                                               |     50      |              |
| `agent.explain.plan_sender_buffer` | explain result buffer                                                                                |     50      |              |

**`metadata`**

```yaml
[ agent_name: <string> | default = "dsk-trace-agent" ]

[ cluster_id: <cluster_id> | default = "unknown" ]
```

**`data_source_name`**

```yaml
[ user: <string> | required ]

[ password: <string> | required ]

[ address: <string> | required ]

[ port: <uint16> | required ]

[ DBName: <string> | required ]
```

**`explain`**

```yaml
[scrape_interval: <seconds> | default=30s] 

[scrape_timeout: <seconds> | default=5s]

[slow_query_standard: <seconds> | default=5s ]

[executor_number: <int8> | default=10 ]

[sender_number: <int8> | default=10 ]

[activity_query_buffer: <int16> | default=50 ]

[plan_sender_buffer: <int16> | default=50 ]
```

### 4. 패키지 실행

```shell
systemctl enable dsk-plan-postgres-agent --now
```

### 5. 패키지 실행 상태 확인

```shell
systemctl status dsk-plan-postgres-agent
```

## Plan Postgres Agent 제거하기

### 1. 패키지 중단

```shell
systemctl stop dsk-plan-postgres-agent
```

### 2. 패키지 제거

```shell
yum remove dsk-plan-postgres-agent
```
