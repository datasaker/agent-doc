# dsk-log-agent

## 쿠버네티스 환경에서 DataSaker Log agent 설치하기

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다. `Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다. 그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다. 또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## DataSaker 선행 작업을 진행하였나요?

현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Log agent 설치하기

기본적으로, `Log agent`는 쿠버네티스 환경에서 기본적으로 데몬셋(daemonset)으로 배포됩니다. 따라서, 모든 노드에 `Log agent`가 설치됩니다. 만약, 특정 노드에만 `Log agent`를 설치하고 싶다면, 해당 노드에 affinity 또는 nodeSelector를 추가적으로 설정해주십시오.

### 1. Log agent 설정 값 등록

`Log agent`의 설정 값의 의미와 기본 설정값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요. "\~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.

다음은 로그 에이전트 구성 파일에서 각각의 설정 항목에 대한 설명입니다.

```yaml
logAgent:
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
        kubernetes:
          - namespace:
            pod:
            container:
```

| **Settings**                        | **Description**                                                                        |   **Default**    |
| ----------------------------------- | -------------------------------------------------------------------------------------- |:----------------:|
| **logs**                          | 로그 수집 대상 정보                                                                        |                  |
| `service`                           | 로그 수집 대상의 서비스 이름                                                              |    `default`     |
| `tag`                             | 로그 수집 대상의 태그                                                                    |                |
| `keyword`                           | 로그 수집 키워드 (키워드가 포함된 로그만 수집)                                             |                  |
| **multiline**                       | 멀티라인 로그 수집 설정                                                                  |                  |
| `format`                  | 멀티라인 로그 포맷 (예 : go, java, ruby, python)                                          |                  |
| `pattern`                 | 멀티라인 로그 패턴 (예 : ^\d{4}-\d{2}-\d{2}) - 사용자 커스텀 정규식 패턴 사용 가능           |                  |
| **masking**                       | 민감 정보 로그 마스킹 설정                                                                |                  |
| `pattern`                           | 마스킹할 로그 패턴 (예 : ^\d{4}-\d{2}-\d{2}) - 사용자 커스텀 정규식 패턴 사용 가능           |                  |
| `replace`                           | 마스킹 패턴이 대체될 문자열 (예 : ******)                                                  |                  |
| **collect**                         | 로그 수집 대상 설정                                                                      |                  |
| `type`                              | 로그 수집 방법 (`file`, `driver`, `kubernetes` 중 하나의 값을 작성)                      |      `file`      |
| `category`                          | 서비스 분류 (`app`, `database`, `syslog`, `etc` 중 하나의 값을 작성)                     |      `etc`       |
| `address`                           | 데이터베이스 host 및 port 정보  (서비스 분류가 database인 경우 설정)                       |                  |
| **file**                            | 로그 수집 방법이 file 인 경우 설정                                                       |                  |
| `paths`                           | 로그 수집 대상 경로 (예 : /var/log/sample/*.log)                                          | `/var/log/*.log` |
| `exclude_paths`                   | 로그 수집 제외 대상 경로                                                                  |                  |
| **kubernetes**                    | 로그 수집 방법이 kubernetes 인 경우 설정                                                   |                  |
| `namespace`                         | 로그 수집 대상 네임스페이스                                                                    |       `*`        |
| `pod`                          | 로그 수집 대상 파드 이름                                                                       |       `*`        |
| `container`                    | 로그 수집 대상 컨테이너 이름                                                                    |       `*`        |

쿠버네티스 환경에서 로그 에이전트는 2가지 방법으로 로그를 수집할 수 있습니다.

* **kubernetes** : 로그를 수집할 워크로드의 정보를 통해 로그를 수집합니다. 수집할 워크로드의 네임스페이스, 파드 이름, 컨테이너 이름을 설정합니다. (로그 에이전트를 Daemonset으로 배포할 경우, 해당 방법을 사용하세요.)
* **file** : 로그 파일을 직접 수집합니다. 수집할 로그를 로그 에이전트에 직접 마운트하고 수집 로그 파일 상대 경로를 설정합니다. (로그 에이전트를 Sidecar패턴으로 배포할 경우, 해당 방법을 사용하세요.)

예를 들어, 쿠버네티스 워크로드 정보를 통해 로그를 수집하는 경우 다음과 같이 구성 파일을 작성할 수 있습니다.

만약 다음과 같은 manifest 파일로 구성된 워크로드가 있다면,

 ```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
 ```

다음과 같이 로그 에이전트 YAML 구성 파일을 작성할 수 있습니다.

```shell
cat << EOF >> ~/datasaker/config.yaml
logAgent:
  enabled: true
  logs:
    - collect:
        type: kubernetes
        kubernetes:
          - namespace: default
            pod: nginx-deployment
            container: nginx
EOF
```

특정 namespace/pod/container 이름의 워크로드에 대해 모든 로그 파일을 수집하고 싶은 경우, 공통된 키워드만 작성하여 해당 키워드가 포함된 모든 워크로드의 로그를 수집할 수 있습니다.

만약, 모든 namespace에 nginx라는 이름이 포함된 pod에 대해 모든 컨테이너의 로그를 수집하고 싶다면, 다음과 같이 구성 파일을 작성할 수 있습니다.
(주의 : namespace/pod/container에 어떠한 값도 작성하지 않으면, 모든 namespace/pod/container에 대해 모든 컨테이너의 로그를 수집합니다.)

```shell
cat << EOF >> ~/datasaker/config.yaml
logAgent:
  enabled: true
  logs:
    - collect:
        type: kubernetes
        kubernetes:
          - pod: nginx
EOF
```

로그 파일을 직접 수집하는 경우는 다음과 같이 구성 파일을 작성할 수 있습니다.

```shell
cat << EOF >> ~/datasaker/config.yaml
logAgent:
  enabled: true
  logs:
    - collect:
        type: file
        file:
          paths:
           - /var/log/containers/nginx-deployment*default*nginx*.log
EOF
```

### 2. Log agent 설치

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker -f ~/datasaker/config.yaml
```
