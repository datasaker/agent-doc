# dsk-log-agent

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다. `Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다. 그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다. 또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## DataSaker 선행 작업을 진행하였나요?

현재 RedHat 9  환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

## Log agent 설치하기

`Log agent` 는 fluent-bit을 통해 로그를 수집하여 에이전트 설치 시 반드시 fluent-bit 2.1.0 버전 이상이 설치되어 있어야 합니다.

fluent-bit 패키지를 설치는 [공식문서](https://docs.fluentbit.io/manual/installation/linux/redhat-centos)를 참고하여 설치합니다.

### 1. 패키지 설치

```shell
sudo yum install dsk-log-agent
```

### 2. agent-config 설정

`/etc/datasaker/dsk-log-agent/agent-config.yml` 경로에 agent-config 파일을 생성합니다.

예를 들어, 로그 파일을 수집하는 경우 다음과 같이 구성 파일을 작성할 수 있습니다.

```yaml
agent:
  logs:
    - collect:
        type: file
        file:
          paths:
            - '/var/log/sample/*.log'
```

> 주의사항 : 로그 파일 이외의 파일이 로그 수집 대상 경로에 설정되지 않도록 설정하십시오. 로그 수집 경로의 파일을 지정하거나 그 이외의 파일을 제외시키십시오. 로그 수집 설정에 파일 확장자 형식을 반드시 작성해주십시오. (예시: `.log`)

### 3. 패키지 실행

```shell
systemctl enable dsk-log-agent --now
```

### 4. 패키지 실행 상태 확인

```shell
systemctl status dsk-log-agent
```

## Log agent 제거하기

### 1. 패키지 중단

```shell
systemctl stop dsk-log-agent
```

### 2. 패키지 제거

패키지 제거 전 반드시 패키지를 중단하고 제거하십시오.

```shell
yum remove dsk-log-agent
```

## Log agent 구성 설정하기

로그 에이전트의 로그 수집 설정을 변경하고자 할 경우, `/etc/datasaker/dsk-log-agent/agent-config.yml` 경로에 구성 파일을 수정하십시오.

다음은 로그 에이전트 구성 파일에서 각각의 설정 항목에 대한 설명입니다.

```yaml
agent:
  metadata:
    agent_name:
    cluster_id:
  logs:
    - service:
      tag: []
      keyword: []
      multiline:
        format:
        pattern: []
      masking:
        - pattern:
          replace:
      collect:
        type:
        category:
        address:
        file:
          paths: []
          exclude_paths: []
```

| **Settings**                        | **Description**                                                                        | **Default** |
| ----------------------------------- | -------------------------------------------------------------------------------------- | :---------: |
|  **metadata**                       | 에이전트 기본 정보                                                                                    |
| `agent_name`                        | 로그 에이전트 이름                                                                       | `dsk-log-agent` |
| `cluster_id`                        | 관제 대상이 되는 환경의 클러스터 정보                                                     |  `unknown`  |
| **logs**                          | 로그 수집 대상 정보                                                                                    |
| `service`                           | 로그 수집 대상의 서비스 이름                                                              |  `default`  |
| `tag`                             | 로그 수집 대상의 태그                                                                    |     N/A     |
| `keyword`                           | 로그 수집 키워드 (키워드가 포함된 로그만 수집)                                             |             |
| **multiline**                       | 멀티라인 로그 수집 설정                                                                                 |
| `format`                  | 멀티라인 로그 포맷 (예 : go, java, ruby, python)                                          |             |
| `pattern`                 | 멀티라인 로그 패턴 (예 : ^\d{4}-\d{2}-\d{2}) - 사용자 커스텀 정규식 패턴 사용 가능           |             |
| **masking**                       | 민감 정보 로그 마스킹 설정                                                                              |
| `pattern`                           | 마스킹할 로그 패턴 (예 : ^\d{4}-\d{2}-\d{2}) - 사용자 커스텀 정규식 패턴 사용 가능           |            |
| `replace`                           | 마스킹 패턴이 대체될 문자열 (예 : ******)                                                  |            |
| **collect**                         | 로그 수집 대상 설정                                                                                    |
| `type`                              | 로그 수집 방법 (`file`, `driver` 중 하나의 값을 작성)                                    |    `file`    |
| `category`                          | 서비스 분류 (`app`, `database`, `syslog`, `etc` 중 하나의 값을 작성)                     |    `etc`    |
| `address`                           | 데이터베이스 host 및 port 정보  (서비스 분류가 database인 경우 설정)                       |          |
| **file**                            | 로그 수집 방법이 file 인 경우 설정                                                                   |
| `paths`                           | 로그 수집 대상 경로 (예 : /var/log/sample/*.log)                                          | `/var/log/*.log` |
| `exclude_paths`                   | 로그 수집 제외 대상 경로                                                                   |          |
