# dsk-rabbitmq-agent
## 도커 환경에 DataSaker RabbitMQ agent 설치하기
`RabbitMQ agent`는 DataSaker에서 RabbitMQ 정보를 수집하는 agent입니다.

## DataSaker 선행 작업을 진행하였나요?
현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## RabbitMQ agent 설치하기
### 1. RabbitMQ agent 설정값 등록
에이전트를 연결하기 위해서는 수집하고자 하는 RabbitMQ의 설정 정보(rabbit_url, rabbit_user, rabbit_pass)를 설정해야 합니다.
```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/rabbitmq.json
{
  "rabbit_url": "http://localhost:15672",
  "rabbit_user": "guest",
  "rabbit_pass": "guest",
  "publish_addr": "",
  "publish_port": "19419",
  "ca_file": "",
  "cert_file": "",
  "key_file": "",
  "rabbit_connection": "direct",
  "output_format": "TTY",
  "insecure_skip_verify": false,
  "exlude_metrics": [],
  "include_exchanges": ".*",
  "skip_exchanges": "^$",
  "include_queues": ".*",
  "skip_queues": "^$",
  "skip_vhost": "^$",
  "include_vhost": ".*",
  "rabbit_capabilities": "no_sort,bert",
  "aliveness_vhost": "/",
  "enabled_exporters": [
    "exchange",
    "node",
    "overview",
    "queue",
    "aliveness"
  ],
  "timeout": 30,
  "max_queues": 0
}
EOF
```

터미널에 다음 명령어를 입력하여 dsk-rabbitmq-agent 설정 파일을 생성합니다.
```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/rabbitmq-config.yml
agent:
  metadata:
    agent_name: dsk-rabbitmq-agent
  option:
    exporter_config:
      command: "/etc/datasaker/target-exporter"
      args:
        - --config-file=/etc/datasaker/dsk-rabbitmq-agent/exporter-config.json
    scrape_configs:
      - job_name: dsk-rabbitmq-agent
        url: localhost:19419
        filtering_configs:
          rule: drop
EOF
```

## 2. RabbitMQ agent 설치
RabbitMQ agent를 설치합니다.
```shell
docker run -d --name dsk-rabbitmq-agent \
    -v /var/datasaker/:/var/datasaker/ \
    -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
    -v ~/.datasaker/rabbitmq-config.yml:/etc/datasaker/dsk-rabbitmq-agent/agent-config.yml:ro \
    -v ~/.datasaker/rabbitmq.json:/etc/datasaker/dsk-rabbitmq-agent/exporter-config.json:ro \
    -e DSK_LOG_LEVEL=INFO \
    --restart=always \
    datasaker/dsk-rabbitmq-agent
```