# 도커 환경에 DataSaker Elasticsearch agent 설치 하기

`Elasticsearch agent`는 DataSaker에서 elasitcsearch 정보를 수집하는 agent입니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Elasticsearch agent 설치하기

## 1. Elasticsearch agent 설정값 등록

에이전트를 연결하기 위해서는 수집하고자 하는 Elasticsearch 서버의 호스트, 포트 주소를 에이전트에 설정해야 합니다.

   ```shell
    DSK_ES_URI=http(s)://<user>:<password>@<host>:<port>
   ```

예를 들어, localhost 9200 포트에 서비스중인 Elasticsearch를 수집하기 위해서는 다음과 같이 설정할 수 있습니다.

   ```shell
    DSK_ES_URI=http://localhost:9200
   ```

터미널에 다음 명령어를 입력하여 dsk-elasticsearch-agent 설정 파일을 생성합니다.

```shell
cd ~
mkdir .datasaker
DSK_ES_URI=http://localhost:9200
cat << EOF > ~/.datasaker/elasticsearch-config.yml
agent:
  metadata:
  # agent_name: my-dsk-node-agent
  # cluster_id: my-cluster
  option:
    exporter_config:
      command: "/etc/datasaker/target-exporter"
      args:
        - --es.uri=${DSK_ES_URI}
        - --es.all
        - --es.cluster_settings
        - --es.indices_settings
        - --es.indices_mappings
        - --es.shards
        - --es.snapshots
    scrape_configs:
      - job_name: dsk-elasticsearch-agent
        url: localhost:19114
        filtering_configs:
          rule: drop
EOF
```

## 2. Elasticsearch agent 설치

1. 데이터세이커가 사용할 로컬 디렉터리를 생성합니다.

   ```shell
    sudo mkdir -p /var/datasaker
    sudo chown -R datasaker:datasaker /var/datasaker/ 
   ```

2. 도커 명령어를 서버에 입력합니다.

   ```shell
   docker run -d --name dsk-elasticsearch-agent\
      -v /var/datasaker/:/var/datasaker/\
      -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
      -v ~/.datasaker/elasticsearch-config.yml:/etc/datasaker/dsk-elasticsearch-agent/agent-config.yml:ro\
      -e DSK_LOG_LEVEL=INFO\
      -e DSK_SUB_KIND=elasticsearch-1\
      --restart=always\
      datasaker/dsk-elasticsearch-agent
   ```
