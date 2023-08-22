# dsk-log-agent

## 도커 환경에 Datasaker Log agent 설치하기

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다. `Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다. 그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다. 또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Log agent 설치하기

Docker 환경에서 로그 에이전트를 통해 로그를 수집하기 위해서는 다음과 같은 작업이 필요합니다.

### 1. 에이전트 실행에 필요한 구성 YAML 파일을 생성합니다.

로그 에이전트는 에이전트 구성 YAML 파일을 통해 로그 수집 설정을 관리합니다. 로그 에이전트 구성 파일에서 각각의 설정 항목에 대한 설명은 다음과 같습니다.

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
        driver:
          containers: []
```

| **Settings**                        | **Description**                                                                 |   **Default**    |
| ----------------------------------- | ------------------------------------------------------------------------------- |:----------------:|
|  **metadata**                       | 에이전트 기본 정보                                                                      |                  |
| `agent_name`                        | 로그 에이전트 이름                                                                      | `dsk-log-agent`  |
| `cluster_id`                        | 관제 대상이 되는 환경의 클러스터 정보                                                     |    `unknown`     |
| **logs**                          | 로그 수집 대상 정보                                                                     |                  |
| `service`                           | 로그 수집 대상의 서비스 이름                                                              |    `default`     |
| `tag`                             | 로그 수집 대상의 태그                                                                    |                |
| `keyword`                           | 로그 수집 키워드 (키워드가 포함된 로그만 수집)                                             |                  |
| **multiline**                       | 멀티라인 로그 수집 설정                                                                  |                  |
| `format`                  | 멀티라인 로그 포맷 (예 : go, java, ruby, python)                                         |                  |
| `pattern`                 | 멀티라인 로그 패턴 (예 : ^\d{4}-\d{2}-\d{2}) - 사용자 커스텀 정규식 패턴 사용 가능           |                  |
| **masking**                       | 민감 정보 로그 마스킹 설정                                                                 |                  |
| `pattern`                           | 마스킹할 로그 패턴 (예 : ^\d{4}-\d{2}-\d{2}) - 사용자 커스텀 정규식 패턴 사용 가능           |                  |
| `replace`                           | 마스킹 패턴이 대체될 문자열 (예 : ******)                                                  |                  |
| **collect**                         | 로그 수집 대상 설정                                                                    |                  |
| `type`                              | 로그 수집 방법 (`file`, `driver` 중 하나의 값을 작성)                                    |      `file`      |
| `category`                          | 서비스 분류 (`app`, `database`, `syslog`, `etc` 중 하나의 값을 작성)                     |      `etc`       |
| `address`                           | 데이터베이스 host 및 port 정보  (서비스 분류가 database인 경우 설정)                       |                  |
| **file**                            | 로그 수집 방법이 file 인 경우 설정                                                    |                  |
| `paths`                           | 로그 수집 대상 경로 (예 : /var/log/sample/*.log)                                         | `/var/log/*.log` |
| `exclude_paths`                   | 로그 수집 제외 대상 경로                                                                  |                  |
| **driver**                   | 로그 수집 방법이 driver 인 경우 설정                                                        |                  |
| `containers`                  | 로그 수집 대상 컨테이너 이름                                                                |       `*`        |

Docker 환경에서 로그 에이전트는 2가지 방법으로 로그를 수집할 수 있습니다.

* **driver** : 도커 로깅 드라이버를 사용하여 로그를 수집합니다. 수집할 컨테이너 이름을 설정합니다. (표준 출력으로 로그가 출력되는 경우, 해당 방법을 사용하세요.)
* **file** : 로그 파일을 직접 수집합니다. 수집할 로그를 로그 에이전트에 직접 마운트하고 수집 로그 파일 상대 경로를 설정합니다. (표준 출력으로 로그가 출력되지 않는 경우, 해당 방법을 사용하세요.)


예를 들어, 로깅 드라이버를 통해 컨테이너 이름이 awesome_saker 의 로그를 수집하는 경우 다음과 같이 구성 파일을 작성할 수 있습니다.

```shell
cat << EOF > ~/.datasaker/log-agent-config.yml
agent:
  logs:
    - collect:
        type: driver
        driver:
          containers:
           - awesome_saker
EOF
```

로그 파일을 직접 수집하는 경우는 다음과 같이 구성 파일을 작성할 수 있습니다.

```shell
cat << EOF > ~/.datasaker/log-agent-config.yml
agent:
  logs:
    - collect:
        type: file
        file:
          paths:
           - /var/lib/docker/containers/*awesome_saker*.log
EOF
```

### 2. docker 명령어를 통해 로그 에이전트를 실행합니다.

* (필수) 로그 에이전트에 필요한 구성 파일을 mount합니다. (global, agent YAML configiuration files)
  * `-v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro`
  * `-v ~/.datasaker/log-agent-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml:ro`
  * **\[주의]** global 및 agent 구성 파일은 반드시 작성해야 합니다. 작성하지 않을 경우, 로그 에이전트가 정상적으로 동작하지 않을 수 있습니다.
* (로깅 드라이버를 통해 수집하는 경우) 포트 포워딩 설정이 필요합니다.
  * `-p [HOST_PORT_NUMBER]:21212`
* (로그 파일을 직접 수집하는 경우) 사용자가 수집하고자 하는 로그를 에이전트에 mount합니다.
  * 다음과 같이 옵션을 설정합니다. `-v [COLLECT LOG PATH]:[LOG AGENT MOUNT PATH]:ro`
    * 해당 옵션에서 ':' 앞의 경로는 로그를 수집할 경로를 작성하고, ':' 뒤의 경로는 로그를 수집할 에이전트의 경로를 작성합니다.
    * `[LOG AGENT MOUNT PATH]` 경로를 기준으로 에이전트 구성 파일의 로그 수집 경로를 작성해야 합니다.

다음은 로그 에이전트 실행 예시입니다.

```shell
docker  run -d --name dsk-log-agent \
  -v /var/datasaker/:/var/datasaker/ \
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro \
  -v ~/.datasaker/log-agent-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml:ro \
  -v /var/lib/docker/containers/:/var/lib/docker/containers/:ro \
  -p 21212:21212 \
  --restart=always \
  datasaker/dsk-log-agent:latest
```

**\[주의]** 로그 파일을 직접 수집하는 경우에는 해당 로그를 에이전트에 마운트해야 합니다.

### 3. (로깅 드라이버를 통해 수집하는 경우) 로그를 수집할 대상의 컨테이너를 실행합니다.

* 로그 수집을 위한 필수 옵션을 설정합니다.
  * `--log-driver=fluentd`
  * `--log-opt fluentd-address=[LOG_AGENT_HOST]:[LOG_AGENT_HOST_PORT_NUMBER]`

**\[주의]** 로깅 드라이버를 통해 로그를 수집하는 경우, 로그를 수집할 대상의 컨테이너를 재실행해야 합니다.

다음은 로그 수집을 위한 컨테이너 실행 예시입니다.

```shell
docker  run --log-driver=fluentd --log-opt fluentd-address=dsk-log-agent:21212 your/application
```

## Log agent 로그 수집 설정하기

`Log agent`는 에이전트 구성 YAML 파일을 통해 다중 라인 로그 수집 및 로그 마스킹 설정을 지원합니다.

### 1. 다중 라인 로그 수집 설정

로그 에이전트 구성 YAML 파일에서 다중 라인 로그 수집 설정을 2가지 방법으로 지원합니다.

* **format** : 다중 라인 로그의 포맷을 설정합니다. 현재, `go`, `java`, `python`의 3가지 포맷을 지원합니다.
* **pattern** : 다중 라인 로그의 패턴을 설정합니다. 사용자 커스텀 정규식 패턴을 사용할 수 있습니다.

```yaml
logs:
  - multiline:
      format:
      pattern: []
```

예를 들어, 다음과 같은 다중 라인 로그가 있다면,

```shell
Dec 14 06:41:08 Exception in thread "main" java.lang.RuntimeException: Something has gone wrong, aborting!
    at com.myproject.module.MyProject.badMethod(MyProject.java:22)
    at com.myproject.module.MyProject.oneMoreMethod(MyProject.java:18)
    at com.myproject.module.MyProject.anotherMethod(MyProject.java:14)
    at com.myproject.module.MyProject.someMethod(MyProject.java:10)
    at com.myproject.module.MyProject.main(MyProject.java:6)
```

`format` 설정을 통해 다음과 같이 다중 라인 로그를 수집할 수 있습니다.

```yaml
logs:
  - multiline:
      format: 'java'
```

**\[주의]** `format`을 통한 다중 라인 로그를 수집 방법이 모든 패턴의 다중 라인 로그를 수집할 수는 없습니다. 정확한 다중 라인 로그 수집을 위해서는 `pattern`을 통한 다중 라인 로그 수집 설정을 권장합니다.

다른 방법인 `pattern`을 통해 다중 라인 로그를 수집할 경우에는 다음과 같이 작성할 수 있습니다.

```yaml
logs:
  - multiline:
      pattern: 
        - '^\w*\s\d{1,2}\s\d{1,2}\:\d{1,2}\:\d{1,2}'
```

다중 라인 로그에 대해 로그 시작 부분의 패턴을 작성하면, 해당 패턴으로 시작하는 로그를 기준으로 다중 라인 로그를 수집합니다.

**\[주의]** 로그 에이전트는 다중 라인 로그 수집에서 두 번째 라인부터는 공백으로 시작하는 것으로 판단합니다.

### 2. 민감 정보 로그 마스킹 설정

로그 에이전트 구성 YAML 파일에서 민감 정보 로그 마스킹 설정을 지원합니다.

* **pattern** : 마스킹할 로그 패턴을 설정합니다. 사용자 커스텀 정규식 패턴을 사용할 수 있습니다.
* **replace** : 마스킹 패턴이 대체될 문자열을 설정합니다.

```yaml
logs:
  - masking:
      - pattern:
        replace:
```

예를 들어, 다음과 같은 로그가 있고,

```shell
2023-08-18 06:35:38.993 GMT [739243] LOG:  statement:
            SELECT * 
            FROM address 
            WHERE id = '1234567890';
```

해당 로그에서 `statement:` 뒤에 오는 쿼리문을 `PRIVATE_QUERY` 문자열로 마스킹하고 싶다면, `pattern`에 다음과 같이 정규식 패턴을 작성하여 마스킹할 수 있습니다.

```yaml
logs:
  - masking:
      - pattern: 'statement: .*'
        replace: 'statement: PRIVATE_QUERY'
```
