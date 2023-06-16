# 도커 환경에 Datasaker Log agent 설치하기

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다.
`Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다.
그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다.
또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Log agent 설치하기

## 1. 에이전트 실행에 필요한 구성 YAML 파일을 생성합니다.

사용자가 수집하고자 하는 로그 `/var/lib/docker/containers/`를 에이전트의 `/var/log/sample/` 경로에 mount할 경우, 다음과 같이 구성 파일을 작성할 수 있습니다.

```shell
cat << EOF > ~/.datasaker/log-agent-config.yml
agent:
  metadata:
    agent_name: 'Sample Service Log Agent'
  collect:
    - paths:
        - '/var/log/sample/*.log'
      exclude_paths:
        - '/var/log/sample/private.log'
      keywords:
        - 'ERROR'
        - 'WARN'
      tag: 'Sample Service'
      service:
        name: 'Sample'
        category: 'database'
        type: 'postgres'
        address: 'my-sample-serivce:5432'
EOF
```
**[주의]** `agent.collect.paths[]` 설정 항목은 마운트한 볼륨 경로를 기준으로 작성해야 합니다.(예: '/var/log/sample/') 해당 항목을 작성하지 않을 경우, 로그 에이전트가 정상적으로 동작하지 않을 수 있습니다. 


로그 에이전트 구성 파일에서 각각의 설정 항목에 대한 설명은 다음과 같습니다.

| **Settings**                        | **Description**                                           | **Default** | **Necessary** |
|:------------------------------------|:----------------------------------------------------------|:-----------:|:------------:|
| `agent.metadata.agent_name`      | 로그 에이전트 이름                                                 |     `dsk-log-agent`     |         |
| `agent.collect.paths[]`      | 로그 수집 대상 경로 (예 : '/var/log/sample/*.log')                              |     N/A     |    **✓**     |
| `agent.collect.exclude_paths[]`      | 로그 수집 제외 대상 경로                              |     N/A     |         |
| `agent.collect.keywords[]`         | 로그 수집 키워드 (키워드가 포함된 로그만 수집합니다.)                                  |     N/A     |              |
| `agent.collect.tag`              | 사용자 설정 태그                                                    |     N/A     |              |
| `agent.collect.service.name`     | 서비스 이름                                                       |  `default`  |              |
| `agent.collect.service.category` | 서비스 분류 (`app`, `database`, `syslog`, `etc` 중 하나의 값을 작성하세요.)                 |    `etc`    |              |
| `agent.collect.service.type`     | 서비스 데이터베이스 종류 및 개발 언어 타입 (`postgres`, `mysql`, `java`, `etc` 중 하나의 값을 작성하세요.)                      |    `etc`    |              |
| `agent.collect.service.address`  | 데이터베이스 host 및 port 정보  (서비스 분류가 database인 경우 설정하세요. 설정하지 않을 경우, 특정 기능을 사용하지 못할 수 있습니다.) |     N/A     |      ⚠️      |


## 2. docker 명령어를 통해 로그 에이전트를 실행합니다.

- 로그 에이전트에 필요한 구성 파일을 mount합니다. (global, agent YAML configiuration files)
  - **[주의]** global 및 agent 구성 파일은 반드시 작성해야 합니다. 작성하지 않을 경우, 로그 에이전트가 정상적으로 동작하지 않을 수 있습니다.
- 사용자가 수집하고자 하는 로그를 에이전트에 mount합니다.
  - **[주의]** 로그 파일을 mount할 경우, 반드시 `-mount.volume=true` 옵션을 설정해야 합니다.

다음은 로그 에이전트 실행 예시입니다.

```shell
dockr  run -d --name dsk-log-agent \
  -v /var/datasaker/:/var/datasaker/ \
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro \
  -v ~/.datasaker/log-agent-config.yml:/etc/datasaker/agent-config.yml:ro \
  -v /var/lib/docker/containers/:/var/log/sample/:ro \
  --restart=always \
  datasaker/dsk-log-agent:latest \
  -mount.volume=true
```

# Log agent 사용 예시

로그 에이전트 구성 파일을 적절하게 사용하여 다양한 방법으로 수집 설정을 할 수 있습니다.
- 서로 다른 경로의 로그를 하나의 서비스 이름으로 수집할 수 있습니다. (`paths` 항목에 여러 로그 파일 경로를 작성하세요.)
- 서로 다른 서비스 `category`의 로그 파일을 하나의 서비스 이름으로 수집할 수 있습니다. (서로 다른 `collect`를 작성한 후, `service.name` 항목을 동일하게 작성하세요.)
- 특정 경로의 모든 로그 파일을 수집할 수 있습니다.(`*`을 사용하여 로그 수집 경로를 설정하세요.) 뿐만 아니라 해당 경로 중 제외시키고 싶은 로그 파일이 있다면, `exclude_paths` 항목에 작성하세요.

다음은 로그 수집 설정 예시입니다.

```yaml
agent:
  metadata:
    agent_name: 'Apple & Banana Service Log Agent'
  collect:
    - paths:
        - '~/datasaker/log/apple/app/*.log'
      keywords: 
        - '400'
        - '500'
      service:
        name: 'Apple'
        category: 'app'
        type: 'java'
    - paths:
        - '~/datasaker/log/apple/database/*.log'
      keywords: 
        - 'ERROR'
      service:
        name: 'Apple'
        category: 'database'
        type: 'postgres'
        address: 'apple-serivce:5432'
    - paths:
        - '~/datasaker/log/banana/app/*.log'
      keywords: 
        - 'ERROR'
      service:
        name: 'Banana'
        category: 'app'
        type: 'etc'
```

```shell
dockr  run -d --name dsk-log-agent \
  -v /var/datasaker/:/var/datasaker/ \
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro \
  -v ~/.datasaker/log-agent-config.yml:/etc/datasaker/agent-config.yml:ro \
  -v ~/var/lib/docker/containers/APPLE-APP-SERVER:~/datasaker/apple/app/:ro \
  -v ~/var/lib/docker/containers/APPLE-DB-SERVER:~/datasaker/apple/database/:ro \
  -v ~/var/lib/docker/containers/BANANA-APP-SERVER:~/datasaker/banana/app/:ro \
  --restart=always \
  datasaker/dsk-log-agent:latest \
  -mount.volume=true
```

