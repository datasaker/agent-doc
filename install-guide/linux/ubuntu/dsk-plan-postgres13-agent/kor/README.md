# Ubuntu 환경에 DataSaker Plan Postgres13 agent 설치하기 (Beta)
'Postgres13 agent'는 데이터베이스의 상태 및 슬로우 쿼리를 실시간으로 수집합니다.
이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.
또한, 슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Plan Postgres13 Agent 설치하기
## 1. agent-config 설정
```bash
sudo dsk-plan-postgres13-agent init ${VAR_CLUSTER_NAME}
cat /etc/datasaker/dsk-plan-postgres13-agent/agent-config.yml
```
결과
```
agent:
  metadata:
    agent_name: "dsk-plan-postgres13-agent"         # replace you want
    cluster_id: ${VAR_CLUSTER_NAME}                 # replace you want
  data_source_name:
    user:                                           # <user_name>
    password:                                       # <user_password>
    address:                                        # <database_address>
    port:                                           # <database_port>
    DBName:                                         # <database_name>
  explain:
    scrape_interval: 30s                            # <activity_session_scrape_time>
    scrape_timeout: 5s                              # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s                         # <slow_query_standard> 
    executor_number: 10                             # <explain executor number>
    sender_number: 10                               # <explain sender number>
    activity_query_buffer: 50                       # <activity query buffer>
    plan_sender_buffer: 50                          # <explain result buffer>
```

## 2. 패키지 설치
sudo 권한이 필요합니다.
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres13-agent
```

## 3. 패키지 실행
```bash
sudo dsk-plan-postgres13-agent start
```

## 4. 패키지 실행 상태 확인
### Running
```bash
sudo dsk-plan-postgres13-agent status
```
### Not Running
```bash
sudo dsk-plan-postgres13-agent status
```

# Plan Postgres13 Agent 제거하기
## 1. 패키지 중단
```bash
sudo dsk-plan-postgres13-agent stop
```

## 2. 패키지 제거
```bash
sudo apt remove dsk-plan-postgres13-agent
```
