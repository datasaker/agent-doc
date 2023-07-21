# dsk-log-agent

## 쿠버네티스 환경에서 DataSaker Log agent 설치하기

`Log agent`는 다양한 환경에서 시스템 또는 애플리케이션에서 생성되는 로그 데이터를 거의 실시간으로 수집하고, 처리 및 전송하는 에이전트입니다. `Log agent`를 통해 여러 대의 서버에서 생성되는 로그 데이터를 중앙 집중적으로 관리하고 분석하기 위해 사용할 수 있습니다. 그에 따라 사용자는 시스템 또는 애플리케이션에서 발생하는 문제를 빠르게 감지하고 대응할 수 있습니다. 또한, 로그 데이터를 분석하여 보안, 성능, 비즈니스 분석 등 다양한 목적으로 활용할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## DataSaker 선행 작업을 진행하였나요?

현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](dsk-log-agent/kor/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Log agent 설치하기

기본적으로, `Log agent`는 쿠버네티스 환경에서 데몬셋(daemonset)으로 배포됩니다. 따라서, 모든 노드에 `Log agent`가 설치됩니다. 만약, 특정 노드에만 `Log agent`를 설치하고 싶다면, 해당 노드에 affinity 또는 nodeSelector를 추가적으로 설정해주십시오.

### 1. Log agent 설정 값 등록

`Log agent`가 정상적으로 동작하기 위해서 반드시 _**collect.workloads**_ 에 **반드시** 하나 이상의 로그를 수집할 워크로드를 설정해야 합니다.

`Log agent`의 설정 값의 의미와 기본 설정값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요. "\~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.

| **Settings**                        | **Description**                                                                        | **Default** | **Required** |
| ----------------------------------- | -------------------------------------------------------------------------------------- | :---------: | :----------: |
| `logAgent.collect.workloads[]`      | 로그 수집 대상 워크로드 이름 (예시 : 'nginx')                                                        |     N/A     |     **✓**    |
| `logAgent.collect.keywords`         | 로그 수집 키워드 (키워드가 포함된 로그만 수집)                                                            |     N/A     |              |
| `logAgent.collect.tag`              | 사용자 설정 태그                                                                              |     N/A     |              |
| `logAgent.collect.service.name`     | 서비스 이름                                                                                 |  `default`  |              |
| `logAgent.collect.service.category` | 서비스 분류 (`app`, `database`, `syslog`, `etc` 중 하나의 값을 작성하세요.)                            |    `etc`    |              |
| `logAgent.collect.service.type`     | 서비스 데이터베이스 종류 및 개발 언어 타입 (`postgres`, `mysql`, `java`, `etc` 중 하나의 값을 작성하세요.)          |    `etc`    |              |
| `logAgent.collect.service.address`  | 데이터베이스 host 및 port 정보 (서비스 분류가 database인 경우 설정하세요. 설정하지 않을 경우, 특정 기능을 사용하지 못할 수 있습니다.) |     N/A     |      ⚠️      |

```shell
cat << EOF >> ~/datasaker/config.yaml

logAgent:
  enabled: true
  logLevel: 'INFO'
  collect:
    - workloads:
      - WORKLOAD_NAME
      service:
        name: SERVICE_NAME
        category: APP
        type: ETC
EOF
```

### 2. Log agent 설치

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker -f ~/datasaker/config.yaml
```

## Log agent 사용 방법

### 1. 반드시 하나 이상의 로그 수집 대상 워크로드 이름(`workloads`)을 입력하십시오

워크로드를 작성하지 않을 경우, `Log agent`가 정상적으로 동작하지 않을 수 있습니다.

```yaml
collect:
  - workloads: # [필수] 수집할 워크로드의 이름을 입력합니다.
      - postgres
```

#### \[ **Workloads** 작성 가이드 ]

워크로드는 쿠버네티스에서 구동되는 애플리케이션을 의미합니다. (워크로드가 단일 컴포넌트이거나 함께 작동하는 여러 컴포넌트이든 관계없이, 쿠버네티스에서는 워크로드를 일련의 Pod 집합 내에서 실행됩니다.) 쿠버네티스에는 다음과 같이 여러 가지 빌트인(built-in) 워크로드 리소스를 제공합니다. (Deployment, Replicaset, StatefulSet, DaemonSet)

`collect.workloads` 에는 수집하고자 하는 워크로드의 이름을 작성하면 해당 로그 파일을 수집합니다. (/var/log/containers/_WORKLOAD\_NAME_.log)

예를 들어, 'app-server' 라는 워크로드 이름으로 Pod가 배포되었을 때 해당 Pod의 Container로그는 Workload 이름과 함께 해쉬값이 더해진 파일명으로 생성됩니다. (app-server-5f4b7f7b4f-2q9qz.log) 해당 로그를 수집하기 위해서는 `collect.workloads` 에 'app-server' 를 작성하면 자동으로 해당 로그를 수집합니다.

### 2. 키워드(`keywords`) 설정에 유의하십시오

`keywords` 설정에 등록한 문자열이 포함된 로그만 수집합니다. 하나 이상의 `keywords`를 설정할 경우 한 개의 `keywords`라도 포함되어 있으면 로그를 수집합니다.

```yaml
keywords: [] # 지정된 키워드가 포환된 로그만 수집하도록 설정합니다.
```

### 3. 로그 수집 대상의 `type`이 `database`인 경우 `address` 설정을 권장합니다

해당 설정을 통해 관련 `database agent` 에서 로그 정보를 맵핑하여 보여드립니다. `address` 설정을 하지 않을 경우, 해당 기능을 사용하지 못할 수 있습니다.

```yaml
service:
  name: custom-service-name 
  category: database
  type: postgres
  address: 0.0.0.0:5432
```
