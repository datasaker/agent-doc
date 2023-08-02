# 도커 환경에 Datasaker Trace agent 설치하기

`Trace agent`는 `Opentelemetry`와 `Jaeger`와 같은 오픈소스 분산 추적 시스템과 연동하여, 애플리케이션의 분산 추적 데이터를 수집합니다. 
이를 통해 애플리케이션 내부의 다양한 서비스 간의 통신을 추적하고, 성능 병목 현상을 식별하여 최적화할 수 있습니다. 
수집된 데이터는 빠르게 처리되어 실시간으로 모니터링 및 분석이 가능합니다.
고객의 요구사항에 맞게 `Trace agent` 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

# Trace agent 설치하기

Trace agent는 내부적으로 다음 포트를 사용하고 있습니다.

| Port  | Protocol | Describe       |
|-------|----------|----------------|
| 6831  | UDP      | thrift-compact |
| 6832  | UDP      | thrift-binary  |
| 14250 | TCP      | jaeger-grpc    |
| 14268 | TCP      | jaeger-http    |
| 4317  | TCP      | otlp-grpc      |
| 4318  | TCP      | otlp-http      |

위에서 필요한 포트를 노출하여 Trace agent를 배포할 수 있습니다. \
예를 들어, otlp 데이터만 받을 때, 다음과 같이 배포될 수 있습니다.

```shell
     docker run -d --name dsk-trace-agent\
       -v /var/datasaker/:/var/datasaker/\
       -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
       -e DKS_LOG_LEVEL=info\
       -p 4317:4317/tcp\
       -p 4318:4318/tcp\
       --restart=always\
       datasaker/dsk-trace-agent
```

# Trace agent 설정하기
필요하다면 아래 문서를 참고하여 trace agent의 설정을 바꿀 수 있습니다.

## Trace agent 설정 값
Trace agent의 설정 값의 의미한 default값은 다음과 같습니다.
사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다.
따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다.
최적의 결과를 위해 에이전트 설정을 조정한 후, 에이전트 배포시 볼륨으로 설정 파일을 마운트하여 사용하세요.
에이전트 기본 설정 파일 경로는 `/etc/datasaker/dsk-trace-agent/agent-config.yml`입니다.
설정 파일에서 값을 추가하거나 수정하세요.

파일의 구조는 아래와 같습니다.

### `agent-config.yml`
```yaml
agent:
  # 에이전트의 메타데이터
  metadata: <metadata>
  # 에이전트의 실행 관련 옵션
  option:
    [ collector_config: <collector_config> ]
	[ reciever_config: <reciever_config> ]
```

#### `metadata`
```yaml
# 에이전트 이름 (별칭)
[ agent_name: <string> | default = "dsk-trace-agent" ]

# 관제 대상이 되는 환경이 어떤 클러스터로 묶여있는지에 대한 설정
[ cluster_id: <cluster_id> | default = "unknown" ]
```

#### `collector_config`
```yaml
# collector에 적용되는 샘플링 비율
# 100 이상일 때 모든 데이터가 수집됩니다
[ sampling_rate: <float> | default = 1 ]
```

#### `receiver_config`
```yaml
# collector로부터 데이터를 받을 포트 번호
[ listen_port: <uint16> | default = 14251 ]

# 각 span에 적용되는 커스텀 태크
[ custom_tags: <map[string]string> | default = "" ]
```

# Application에 Trace Agent 연동하기

연동을 위해선 대상 어플리케이션에 다음 환경변수 설정이 필요합니다.

| 환경변수                               | 값                                           | 설명                 |
|------------------------------------|---------------------------------------------|--------------------|
| OTEL_EXPORTER_OTLP_TRACES_ENDPOINT | (trace-agent):(port)                        | trace-agent 주소     |
| OTEL_SERVICE_NAME                  | your-service-name                           | 화면에 표시되길 원하는 서비스 명 |
| OTEL_METRICS_EXPORTER              | none                                        | 불필요 메트릭데이터 생성 방지용  |
| OTEL_LOGS_EXPORTER                 | none                                        | 불필요 로그데이터 생성 방지용   |
| OTEL_RESOURCE_ATTRIBUTES           | dsk_host_key=$(cat /var/datasaker/host_key) | -                  |

예를 들어, 다음과 같이 어플리케이션을 배포할 수 있습니다.

```shell
docker run my-java-application \
    -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=<trace-agent-address>:4317 \
    -e OTEL_SERVICE_NAME=your-service-name \
    -e OTEL_METRICS_EXPORTER=none \
    -e OTEL_LOGS_EXPORTER=none \
    -e OTEL_RESOURCE_ATTRIBUTES=dsk_host_key=$(cat /var/datasaker/host_key) \
    -d
```

더 자세한 정보를 위해서는 다음 내용을 참고하세요.

[관련 문서 링크](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)
