# dsk-elasticsearch-agent

## 도커 환경에 DataSaker Elasticsearch agent 설치 하기

`Elasticsearch agent`는 DataSaker에서 elasitcsearch 정보를 수집하는 agent입니다.

## DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](dsk-elasticsearch-agent/kor/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Elasticsearch agent 설치하기

### 1. Elasticsearch agent 설정값 등록

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
  # agent_name: dsk-elasticsearch-agent
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

위 설정에서 각 argument는 다음을 의미합니다.

| Argument             | Description                                                                                                                                                                                                                                    | Default               |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| es.uri               | 연결해야 하는 Elasticsearch 노드의 주소(호스트 및 포트). 이 주소는 로컬 노드(예: localhost:9200)일 수도 있고 원격 Elasticsearch 서버의 주소일 수도 있습니다. 기본 인증이 필요한 경우, 다음과 같이 지정합니다: ://<사용자>:<비밀번호>@<호스트>:<포트>. 예: http://admin:pass@localhost:9200. 사용자 자격 증명의 특수 문자는 URL 인코딩해야 합니다. | http://localhost:9200 |
| es.all               | true이면 연결한 노드만 쿼리하는 것이 아니라 클러스터의 모든 노드에 대한 통계를 쿼리합니다.                                                                                                                                                                                          | false                 |
| es.cluster\_settings | true이면 클러스터 설정에 대한 통계를 쿼리합니다.                                                                                                                                                                                                                  | false                 |
| es.indices\_settings | true이면 클러스터의 모든 인덱스에 대한 설정 통계를 쿼리합니다.                                                                                                                                                                                                          | false                 |
| es.indices\_mappings | true이면 클러스터의 모든 인덱스 매핑에 대한 통계를 쿼리합니다.                                                                                                                                                                                                          | false                 |
| es.shards            | true인 경우, 샤드 수준 통계를 포함하여 클러스터의 모든 인덱스에 대한 통계를 쿼리합니다.                                                                                                                                                                                           | false                 |
| es.snapshots         | true이면 클러스터 스냅샷에 대한 통계를 쿼리합니다.                                                                                                                                                                                                                 | false                 |

### 2. Elasticsearch agent 설치

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
