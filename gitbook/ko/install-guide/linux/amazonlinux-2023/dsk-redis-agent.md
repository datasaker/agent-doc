# dsk-redis-agent

`redis agent`는 레디스의 상태를 실시간으로 수집합니다.

## Datasaker 선행 작업을 진행하였나요?

현재 환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Redis agent 설치하기

### 패키지 설치

```shell
yum install dsk-redis-agent
```

### 1. Redis agent 설정

```shell
vi /etc/datasaker/dsk-redis-agent/agent-config.yml
```

필요에 따라 다음 내용을 수정합니다.

#### `agent-config.yml`

다음 값은 반드시 수정되어야 합니다.

|     설정      |                                 설명                                  |
|:-----------:|:-------------------------------------------------------------------:|
| $REDIS_ADDR |                     관제하려는 database의 주소를 입력합니다.                      |
| $REDIS_USER | 관제하려는 database의 접속권한을 가진 계정 정보를 입력합니다. (없으면 입력하지 않습니다. 콜론(:)을 지웁니다) |
| $REDIS_PASS |              관제하려는 database의 접속권한을 가진 계정의 비밀번호를 입력합니다.              |

```yaml
agent:
  metadata:
    agent_name: dsk-redis-agent
  option:
    external_labels:
      redis: $REDIS_ADDR
    exporter_config:
      command: "/usr/bin/dsk-redis-exporter"
      port: 19121
      args:
        - --redis.addr=redis://$REDIS_USER:$REDIS_PASS@REDIS_ADDR
    scrape_configs:
      - job_name: dsk-redis-agent
        url: "localhost:19121"
```

### 2. 패키지 실행

```shell
systemctl enable dsk-redis-agent --now
```

### 3. 패키지 실행 상태 확인

```shell
systemctl status dsk-redis-agent
```

## Redis agent 제거하기

### 1. 패키지 중단

```shell
systemctl stop dsk-redis-agent
```

### 2. 패키지 제거

```shell
yum remove dsk-redis-agent
```
