# dsk-log-agent

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다. `Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다. 그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다. 또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## DataSaker 선행 작업을 진행하였나요?

현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업]($%7BPREPARATION\_MANUAL\_KR%7D/)

## Log agent 설치하기

`Log agent`는 현재 Ubuntu 18.04.6 LTS (Bionic Beaver) 환경에서만 지원하고 있습니다.

### 1. 패키지 설치

`DataSaker`의 `Log agent`를 설치하기 위해서는 sudo 권한이 필요합니다.

```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/dsk-log-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-log-agent
```

### 2. agent-config 설정

`/etc/datasaker/dsk-log-agent/agent-config.yml` 경로에 agent-config 파일을 다음과 같이 생성합니다.

```yaml
agent:
  metadata:
    agent_name: 'dsk-log-agent'
  collect:
    - paths:
      - '/var/log/sample/*.log'
      exclude_paths:
      - '/var/log/sample/private.log'
      keywords:
      - 'ERROR'
      tag: 'sample'
      service:
        name: 'sample service'
        category: 'database'
        type: 'postgres'
        address: '0.0.0.0:5432'
```

#### **\[주의]**

1. 반드시 하나 이상의 로그 수집 경로(`path`)를 입력하십시오. 로그 수집 경로를 작성하지 않을 경우, `Log agent`가 정상적으로 동작하지 않을 수 있습니다.
2. 다음과 같이 특정 경로의 모든 로그를 수집하도록 설정할 경우 로그 에이전트에 많은 부하가 발생할 수 있습니다. (`/var/log/*`) 수집 로그 파일을 개별적으로 작성하는 것을 권장합니다.(`/var/log/containers/sampleApp.log`)
3. 로그 파일 이외의 파일이 경로에 설정되지 않도록 설정하십시오. 로그 수집 경로의 파일을 지정하거나 그 이외의 파일을 제외시키십시오. 로그 수집 설정에 포맷을 반드시 작성해주십시오. (예시: `.log`)

로그 에이전트 구성 파일에서 각각의 설정 항목에 대한 설명은 다음과 같습니다.

| **Settings**                     | **Description**                                                                        |   **Default**   | **Necessary** |
| -------------------------------- | -------------------------------------------------------------------------------------- | :-------------: | :-----------: |
| `agent.metadata.agent_name`      | 로그 에이전트 이름                                                                             | `dsk-log-agent` |               |
| `agent.collect.paths[]`          | 로그 수집 대상 경로 (예 : /var/log/sample/\*.log)                                               |       N/A       |     **✓**     |
| `agent.collect.exclude_paths[]`  | 로그 수집 제외 대상 경로                                                                         |       N/A       |               |
| `agent.collect.keywords[]`       | 로그 수집 키워드 (키워드가 포함된 로그만 수집합니다.)                                                        |       N/A       |               |
| `agent.collect.tag`              | 사용자 설정 태그                                                                              |       N/A       |               |
| `agent.collect.service.name`     | 서비스 이름                                                                                 |    `default`    |               |
| `agent.collect.service.category` | 서비스 분류 (`app`, `database`, `syslog`, `etc` 중 하나의 값을 작성하세요.)                            |      `etc`      |               |
| `agent.collect.service.type`     | 서비스 데이터베이스 종류 및 개발 언어 타입 (`postgres`, `mysql`, `java`, `etc` 중 하나의 값을 작성하세요.)          |      `etc`      |               |
| `agent.collect.service.address`  | 데이터베이스 host 및 port 정보 (서비스 분류가 database인 경우 설정하세요. 설정하지 않을 경우, 특정 기능을 사용하지 못할 수 있습니다.) |       N/A       |       ⚠️      |

### 3. 패키지 실행

```shell
sudo systemctl enable dsk-log-agent --now
```

> 주의사항\
>
>
> * 로그 수집을 위해 패키지 실행 전 에이전트 설정 파일을 구성해야 합니다(`global-config.yml`, `config.conf`).\
>
> * 로그 에에전트는 기본적으로 `fluentd`를 사용하여 로그를 수집합니다. 이미 사용 중인 `fluentd`가 있는 경우 `DSK_FLUENTD_CMD` 환경변수를 설정하여 변경할 수 있습니다. 환경변수를 사용하여 실행할 경우 다음과 같이 실행하십시오. (`sudo -E dsk-log-agent start`)

### 4. 패키지 실행 상태 확인

```shell
systemctl status dsk-log-agent
```

또는

```shell
service dsk-log-agent status
```

***

## Log agent 제거하기

### 1. 패키지 중단

```shell
systemctl stop dsk-log-agent
```

또는

```shell
service dsk-log-agent stop
```

### 2. 패키지 제거

패키지 제거 전 반드시 패키지를 중단하고 제거하십시오.

```shell
sudo apt remove dsk-log-agent
```
