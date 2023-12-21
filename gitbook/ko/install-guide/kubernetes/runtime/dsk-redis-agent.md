# dsk-redis-agent

## 쿠버네티스 환경에서 DataSaker Redis agent 설치하기

`redis agent`는 Redis를 관제하는 에이전트입니다.

## DataSaker 선행 작업을 진행하였나요?

현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## redis agent 설치하기

### 1. redis agent 설정 값 등록

```shell
cat << EOF >> ~/datasaker/config.yaml
redisAgents:
  list:
    - name: 'redis-1'
      address: IP:port or Domain
      tls: false
      user: $REDIS_USER_NAME
      password: $REDIS_PASSWORD
EOF
```

각 설정 값에 대한 자세한 설명은 다음과 같습니다.

| 설정                        | 설명                                                        |
|---------------------------|-----------------------------------------------------------|
| `redisAgents[].name`      | `redis agent`를 구분할 수 있는 이름 (반드시 사용자가 명시해야 합니다.)           |
| `redisAgents[].toleration | `redis agent`를 배포할 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다. |
| `redisAgents[].address`   | `redis agent`가 모니터링할 redis의 주소를 설정합니다. (address는 필수값입니다)  |
| `redisAgents[].tls`       | `redis agent`가 모니터링할 redis의 tls 설정 여부를 설정합니다.             |
| `redisAgents[].user`      | `redis agent`가 모니터링할 redis의 user를 설정합니다.                  |
| `redisAgents[].password`  | `redis agent`가 모니터링할 redis의 password를 설정합니다.              |
| `redisAgents[].logLevel`  | `redis agent`의 로그 레벨을 정합니다                                |

### 2. redis agent 설치

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
