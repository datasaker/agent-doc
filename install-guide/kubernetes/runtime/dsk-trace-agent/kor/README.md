# 쿠버네티스 환경에서 DataSaker Trace agent 설치하기
`Trace agent`는 `Opentelemetry`와 `Jaeger`와 같은 오픈소스 분산 추적 시스템과 연동하여, 애플리케이션의 분산 추적 데이터를 수집합니다. 
이를 통해 애플리케이션 내부의 다양한 서비스 간의 통신을 추적하고, 성능 병목 현상을 식별하여 최적화할 수 있습니다. 
수집된 데이터는 빠르게 처리되어 실시간으로 모니터링 및 분석이 가능합니다. 
고객의 요구사항에 맞게 `Trace agent` 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

# Trace agent 설치하기
## 1. Trace agent 설정 값 등록
```shell
cat << EOF >> ~/datasaker/config.yaml

traceAgent:
  enabled: true
  logLevel: 'INFO'
EOF
```

## 2. Trace agent 설치
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

# Trace agent 포트 정보
| Port  | Protocol | Describe       |
|-------|----------|----------------|
| 6831  | UDP      | thrift-compact |
| 6832  | UDP      | thrift-binary  |
| 14250 | TCP      | jaeger-grpc    |
| 14268 | TCP      | jaeger-http    |
| 4317  | TCP      | otlp-grpc      |
| 4318  | TCP      | otlp-http      |

<!--
# 주의 사항

> 기본적으로, Trace agent는 데몬셋으로 배포됩니다. 따라서, 모든 노드에 Trace agent가 설치됩니다. \
> 만약, 특정 노드에만 Trace agent를 설치하고 싶다면, 해당 노드를 위한 afiinity나 nodeSelector를 설정해주시기 바랍니다. \
> 다만, opentelemetry가 연동된 애플리케이션은 Trace agent가 설치된 노드에서만 데이터를 정상적으로 송신 할 수 있으므로 주의하시기 바랍니다.
-->

# Trace agent 설정하기

## Trace agent 설정 값
Trace agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요. 각 항목에 설정된 값은 기본 설정 값입니다.

```yaml
traceAgent:
  # false로 설정하면, 다른 값들은 무시됩니다.
  enabled: false
  tolerations: []
  # 에이전트 이미지를 가져올 때 사용할 이미지 풀 정책을 설정합니다.
  imgPolicy: 'Always' # [Always, IfNotPresent, Never]
  # 에이전트 이미지의 버전을 설정합니다.
  imgVersion: 'latest'
  # tags는 트레이스 에이전트에 대한 커스텀 태그이며 key=value,key2=value2 형식으로 작성할 수 있습니다.
  tags: []
  # resources는 트레이스 에이전트의 리소스 제한을 설정합니다.
  resource:
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1000m
      memory: 1000Mi
  # logLevel은 에이전트의 로그 레벨을 설정합니다.
  logLevel: 'INFO'
  # nodeSelector를 설정하면 특정 노드에만 에이전트가 배포됩니다.
  nodeSelector: {}
  # affinity는 에이전트의 affinity 설정입니다.
  affinity: {}
  # collector 설정은 Trace 데이터 수집과 관련된 설정입니다.
  collector:
    # sampleRate는 Trace 데이터를 수집할 확률을 설정합니다. 100 이상이면 Trace 데이터를 전부 수집합니다.  (0 < sampleRate <= 100)
    samplingRate: 1
```

각 설정 값에 대한 자세한 설명은 다음과 같습니다.

| 설정 값        | 설명                                                                     |
|-------------|------------------------------------------------------------------------|
| enabled     | Trace agent를 활성화를 설정합니다.                                               |
| tolerations | 워커 노드에 taint가 설정되어 있을 경우 toleration 설정을 추가합니다.                         |
| imgPolicy   | agent의 Image Policy를 설정합니다. (Always, IfNotPresent, Never)              |
| imgVersion  | agent의 Image Version를 설정합니다.                                           |
| logLevel    | agent의 log level을 설정합니다. (debug > info > warn > error > panic > fatal) |
| tags        | 사용자 설정으로 데이터에 추가할 Tag 리스트를 입력하세요. key1=value1,key2=value2              |
| resource    | agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.                     |

# Application에 Trace Agent 연동하기

[관련 문서 링크](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)
