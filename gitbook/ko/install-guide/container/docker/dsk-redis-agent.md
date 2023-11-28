# dsk-redis-agent

## 도커 환경에 DataSaker Redis agent 설치 하기

`Redis agent`는 고객이 사용하는 redis를 모니터링하는 에이전트이빈다.

## DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Redis agent 설치하기

Cluster ID를 설정하면 자원을 논리적인 그룹으로 관리할 수 있습니다. Cluster ID를 설정하지 않으면 기본값으로 `unknown`이 설정됩니다.

Cluster ID를 설정하기 위해선 다음과 같이 환경 변수를 설정해야 합니다.

```shell
export DSK_CLUSTER_ID=my-cluster
```

1. 데이터세이커가 사용할 로컬 디렉터리를 생성합니다.

```shell
   sudo mkdir -p /var/datasaker
   sudo chown -R datasaker:datasaker /var/datasaker/
```

2. redis agent가 사용 할 설정 파일을 생성합니다.

아래 설정 예시 중, `$` 표기된 부분을 적절하게 변경해주세요

```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/redis-config.yml
agent:
  metadata:
    agent_name: dsk-redis-agent
  option:
    external_labels:
      redis: $REDIS_ADDR
    exporter_config:
      command: "/etc/datasaker/target-exporter"
      args:
        - --redis.addr=redis://$REDIS_USER:$REDIS_PASS@REDIS_ADDR
    scrape_configs:
      - job_name: dsk-redis-agent
        url: "localhost:19121"
EOF
```

3. 도커 명령어를 서버에 입력합니다.

```shell
 docker run -d --name dsk-redis-agent\
  -v /var/datasaker/:/var/datasaker/\
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
  -v ~/.datasaker/redis-config.yml:/etc/datasaker/dsk-redis-agent/agent-config.yml:ro\
  --restart=always\
  datasaker/dsk-redis-agent
```

### 제한 사항

센티넬 모드로 동작하는 redis는 현재 지원하지 않습니다.