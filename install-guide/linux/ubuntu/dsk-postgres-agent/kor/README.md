# Ubuntu 환경에 DataSaker Postgres agent 설치하기
'Postgres agent'는 데이터베이스의 상태 및 슬로우 쿼리를 실시간으로 수집합니다.
이를 통해 데이터베이스의 성능 지표, 리소스 사용량, 슬로우 쿼리 등 다양한 정보를 수집할 수 있습니다.
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.
또한, 슬로우 쿼리를 탐지하여 인덱스 생성, 쿼리 최적화 등의 방법으로 데이터베이스 성능을 개선할 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](https://github.com/datasaker/documentation/tree/main/install-guide/linux/ubuntu)

# Postgres agent 설치하기
## 1. agent-config 설정
`/etc/datasaker/dsk-postgres-agent/agent-config.yml`에 내용을 기입합니다.
```yaml
agent:
  metadata:
  # agent_name: my-dsk-postgres-agent
  # cluster_id: my-cluster
  option:
    exporter_config:
      command: "/usr/bin/dsk-postgres-exporter"
      port: 19187
      args:
        - --extend.query-path=/etc/datasaker/dsk-postgres-agent/queries.yaml
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-postgres-agent
        url: localhost:19187
        filtering_configs:
          rule: drop
```

## 2. 패키지 설치
```bash
> DSK_GLOBAL_API_KEY=${VAR_GLOBAL_APIKEY}
> curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/M2FyX83ZcA6MZkW/download/dsk-postgres-agent-install.sh
> chmod 700 installer.sh
> sudo ./installer.sh

> dsk-postgres-agent status
```
## 3. 패키지 실행
```bash
$ sudo -E dsk-postgres-agent start
Agent is running
```

## 4. 패키지 실행 상태 확인
### Running
```bash
$ sudo dsk-postgres-agent status
Agent is running
```
### Not Running
```bash
$ sudo dsk-postgres-agent status
Agent is not running
```

# Postgres agent 제거하기
## 1. 패키지 중단
```bash
$ sudo dsk-postgres-agent stop
```

## 2. 패키지 제거
```bash
sudo apt remove dsk-postgres-agent
```