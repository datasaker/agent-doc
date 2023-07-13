# dsk-trace-agent

`Trace agent`는 opentelemetry와 Jaeger와 같은 오픈소스 분산 추적 시스템과 연동하여, 애플리케이션의 분산 추적 데이터를 수집합니다.
이를 통해 애플리케이션 내부의 다양한 서비스 간의 통신을 추적하고, 성능 병목 현상을 식별하여 최적화할 수 있습니다.
수집된 데이터는 빠르게 처리되어 실시간으로 모니터링 및 분석이 가능합니다.
고객의 요구사항에 맞게 "Trace Agent" 설정을 조정하여 최적의 결과를 제공해 드립니다.

# Datasaker 선행 작업을 진행하였나요?
현재 RedHat 7  환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Trace agent 설치하기
## 1. 패키지 설치
```shell
sudo yum install dsk-trace-agent
```

## 2. Trace agent 설정
```shell
sudo vi /etc/datasaker/dsk-trace-agent/agent-config.yml
```
필요에 따라 다음 내용을 수정합니다.

```yaml
# Trace agent 설정 파일
agent:
  metadata:
    agent_name: "dsk-trace-agent" # 에이전트 이름 (별칭) default=dsk-trace-agent
```

## 3. 패키지 실행
```shell
sudo systemctl enable dsk-trace-agent --now
```

## 4. 패키지 실행 상태 확인
```shell
sudo systemctl status dsk-trace-agent
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

# Application에 Trace agent 연동하기
Trace agent를 이용하기 위해서는 먼저 SDK로 어플리케이션을 인스트로먼트 해야 합니다.
자세한 내용은 링크를 참고해주세요.
[관련 문서 링크](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)

# Trace agent 설정하기
필요하다면 아래 문서를 참고하여 trace agent의 설정을 바꿀 수 있습니다.

## Trace agent 설정 값
Trace agent의 설정 값의 의미한 default값은 다음과 같습니다.
사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다.
따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다.
최적의 결과를 위해 에이전트 설정을 조정하세요.
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

### Example
```yaml
agent:
  metadata:
    agent_name: dsk-trace-agent # 에이전트 이름 (별칭) default=dsk-trace-agent
  option:
    collector_configs:
      sampling_rate: 1
```

# Trace agent 제거하기
## 1. 패키지 중단
```shell
sudo systemctl stop dsk-trace-agent
```

## 2. 패키지 제거
```shell
sudo systemctl stop dsk-trace-agent &&
sudo yum remove dsk-trace-agent
```
