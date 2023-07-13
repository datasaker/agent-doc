# 쿠버네티스 환경에서 DataSaker Base agent 설치하기

`Base agent`는 서버에서 발생하는 다양한 정보를 실시간으로 수집합니다.
예를 들어, 메모리, CPU 사용량 등 서버의 성능 지표, 네트워크 트래픽, Container 정보 등 다양한 정보를 수집할 수 있습니다.
이를 통해 고객은 서버의 상태를 실시간으로 모니터링할 수 있으며, 이를 기반으로 서버의 성능을 최적화하고 안정성을 높일 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Base agent 설치하기
## 1. Base agent 설정 값 등록

```shell
cat << EOF >> ~/datasaker/config.yaml

baseAgent:
  enabled: true
  enableMaster: true
  nodeAgent:
    logLevel: 'INFO'
  containerAgent:
    logLevel: 'INFO'
EOF
```

각 설정 값에 대한 자세한 설명은 다음과 같습니다.

| 설정                                  | 설명                                                 |
|-------------------------------------|----------------------------------------------------|
| `baseAgent.enabled`                 | `base agent`를 활성화할지 여부를 결정합니다.                     |
| `baseAgent.enableMaster`            | `base agent`를 `control-plane` 노드에도 배포할지 여부를 결정합니다. |
| `baseAgent.nodeAgent.logLevel`      | `node agent`의 로그 레벨을 설정합니다.                        |
| `baseAgent.containerAgent.logLevel` | `container agent`의 로그 레벨을 설정합니다.                   |

## 2. Base agent 설치

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

# 트러블슈팅

## 포트 사용 이슈

Base Agent는 모니터링을 위해, 설치 환경에서 두 개의 포트를 사용합니다. (default=`14194`,`19110`) 해당 포트를 서버의 다른 프로그램이 이미 점유중일때, Base agent는 정상적으로 기동하지 못하며 다음과 같은 로그를 출력합니다.

```shell
ts=2023-03-14T02:54:49.460Z caller=node_exporter.go:201 level=error err="listen tcp :19110: bind: address already in use"
{"level":"error","ts":"2023-03-14T02:54:49Z","msg":"error occurred from subprocess","error":"error occured during running child process. err: exit status 1"}
{"level":"error","ts":"2023-03-14T02:54:49Z","msg":"error occurred from subprocess","error":"exporter exit unexpectedly and retry count exceeded"}
{"level":"fatal","ts":"2023-03-14T02:54:49Z","msg":"child process is terminated unexpectedly, please check other logs","error":"exporter exit unexpectedly and retry count exceeded"}
```

이 문제를 해결하기 위해서, config.yaml 파일에 `listenPort` 설정을 추가할 수 있습니다.

예를들어, `base agent` 내부의 `node agent`가 사용하는 19110 포트를 19111로 바꾸기 위해 다음과 같이 설정을 수정할 수 있습니다.

```yaml
baseAgent:
  enabled: true
  enableMaster: true
  nodeAgent:
    logLevel: 'INFO'
    listenPort: 19111
  containerAgent:
    logLevel: 'INFO'
```
