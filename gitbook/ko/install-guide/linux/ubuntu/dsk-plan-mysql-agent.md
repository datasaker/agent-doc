# dsk-plan-mysql-agent

'Mysql agent'는 데이터베이스의 상태 및 슬로우 쿼리를 실시간으로 수집합니다. 이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다. 수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다. 또한, 슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## Supported version

| version      | support |
| ------------ | ------- |
| MySQL 8.0.33 | O       |

## DataSaker 선행 작업을 진행하였나요?

현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업]($%7BPREPARATION\_MANUAL\_KR%7D/)

## Plan MySQL Agent install

### 1. MySQL 설정 변경

관제하려는 데이터베이스 `performance_schema=ON` 모듈의 활성화 된 상태인지 확인 부탁드립니다.\
[performance\_schema 참조사이트](https://dev.mysql.com/doc/refman/8.0/en/performance-schema-quick-start.html)

### 2. MySQL User 권한 설정

`MySQL agent`를 설치하기 위해서는 `MySQL User`에 권한을 부여해야 합니다.\
`MySQL user`의 권한을 확인하고, 권한이 없다면 권한을 부여해주세요.\
필요한 User 권한은 다음과 같습니다.

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[MySQL user 권한 참조사이트](https://dev.mysql.com/doc/refman/8.0/en/grant.html)

### 3. 패키지 설치

```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-plan-mysql-agent
```

### 4. MySQL agent 설정값 등록

#### 필수입력 항목

필수입력 항목은 다음과 같습니다. MySQL 설정에 맞게 값을 넣어주세요

| Entity                            | Description             |
| --------------------------------- | ----------------------- |
| agent.data\_source\_name.user     | MySQL User 아이디를 입력합니다.  |
| agent.data\_source\_name.password | MySQL User 패스워드를 입력합니다. |
| agent.data\_source\_name.address  | MySQL 주소를 입력합니다.        |
| agent.data\_source\_name.port     | MySQL 포트를 입력합니다.        |
| agent.data\_source\_name.DBName   | MySQL 데이터베이스 이름을 입력합니다. |

#### 옵션입력

```shell
vi /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml
```

필요에 따라 다음 내용을 수정합니다.

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-mysql-agent"
  data_source_name:
    user: 'user'
    password: 'pass'
    address: '127.0.0.1'
    port: '3306'
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

```bash
sudo dsk-plan-mysql-agent start
```

### 6. 패키지 실행 상태 확인

#### Running

```bash
systemctl status dsk-plan-mysql-agent
```

또는

```shell
serivce dsk-plan-mysql-agent
```

## Log agent 제거하기

### 1. 패키지 중단

```bash
sudo dsk-plan-mysql-agent stop
```

### 2. 패키지 제거

```bash
sudo apt remove dsk-plan-mysql-agent
```
