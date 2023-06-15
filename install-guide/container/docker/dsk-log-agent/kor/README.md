# 도커 환경에 Datasaker Log agent 설치하기

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다.
`Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다.
그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다.
또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Log agent 설치하기

1. 데이터세이커 에이전트에 필요한 구성 파일들을 mount합니다. (global, agent configiuration files)

2. 로그 에이전트가 수집할 로그 파일을 mount합니다. (log files)

3. mount 경로에 맞게 로그 에이전트 flag 옵션을 설정합니다.

다음은 로그 에이전트 실행 예시입니다.

```shell
dockr  run -d --name dsk-log-agent \
  -v /var/datasaker/:/var/datasaker/ \
  -v ~/.datasaker/global-config.yml:/etc/datasaker/global-config.yml:ro \
  -v ~/.datasaker/agent-config.yml:/etc/datasaker/agent-config.yml:ro \
  --restart=always \
  datasaker/dsk-log-agent:latest \
  -global.config=/etc/datasaker/global-config.yml \
  -agent.config=/etc/datasaker/agent-config.yml \
  -mount.volume=true
```

# Log agent 사용 방법

## 1. 로그 에이전트 실행 시 필수 flag 옵션을 설정해주십시오.

- `-global.config` : global 설정 파일 경로
- `-agent.config` : agent 설정 파일 경로
- `-mount.volume` : 로그 파일을 mount할 경우, true로 설정

## 2. 반드시 하나 이상의 로그 수집 경로(path)를 입력하십시오.

로그 수집 경로를 작성하지 않을 경우, Log agent가 정상적으로 동작하지 않을 수 있습니다.

```yaml
collect:
  - paths: []         # (필수) 로그 수집 경로
    exclude_paths: [] # 로그 수집 경로 중 제외시키고자 하는 로그 경로
    keywords: []      # 로그 수집 키워드 (키워드가 포함된 로그만 수집)
    tag:              # 사용자 설정 태그
    service:
      name:           # 서비스 이름  (기본 설정값: default)
      category:       # 서비스 분류  [app, database, syslog, etc] (기본 설정값: etc)
      type:           # 서비스 소스 타입 [postgres, mysql, java] (기본 설정값: etc)
      address:        # 사용자 설정 - 데이터베이스 host 및 port 정보 (type이 database 인 경우 작성)
```

[주의]

다음과 같이 특정 경로의 모든 로그를 수집하도록 설정할 경우 로그 에이전트에 많은 부하가 발생할 수 있습니다. (/var/log/*) 수집 로그 파일을 개별적으로 작성하는 것을 권장합니다.(/var/log/containers/sampleApp.log)


## 3. 수집하고자 하는 로그 파일이 있는 볼륨을 로그 에이전트에 mount하십시오.

로그 에이전트가 수집할 로그 파일이 있는 볼륨을 mount하십시오. (예: /var/lib/docker/containers/*.log)

```shell
docker run -d --name dsk-log-agent \
  ... \
  -v /var/lib/docker/containers/:~/datasaker/log/:ro \
  datasaker/dsk-log-agent:latest
```

에이전트 설정 파일에서 로그 수집 경로를 마운트한 경로를 기준으로 다음과 같이 작성합니다.

```yaml
collect:
  - paths:
      - ~/datasaker/log/*.log
```
