# 도커 환경에 DataSaker Mongo agent 설치하기

`Mongo agent`를 도커 환경에 설치하는 방법을 설명합니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Mongo Agent 설치하기

## 1. Mongo agent 설정값 등록

에이전트를 연결하기 위해서는 수집하고자 하는 몽고DB 서버의 호스트, 포트 주소를 에이전에에 설정해야 합니다.

   ```shell
    DSK_MONGO_URI=mongodb://<user>:<password>@<host>:<port>
   ```

예를 들어, 주소가 `192.168.123.132`이고, `27017` 포트에 서비스중인 몽고DB를 수집하기 위해서는 다음과 같이 설정할 수 있습니다.

   ```shell
    DSK_MONGO_URI=mongodb://192.168.123.132:27017
   ```

터미널에 다음 명령어를 입력하여 dsk-mongo-agent 설정 파일을 생성합니다.

```shell
cd ~
mkdir .datasaker
DSK_MONGO_URI=mongodb://localhost:27017
cat << EOF > ~/.datasaker/mongo-config.yml
agent:
  metadata:
    agent_name: dsk-mongo-agent
  option:
    exporter_config:
      command: "/etc/datasaker/target-exporter"
      args:
        - --collect-all
        - --mongodb.uri=${DSK_MONGO_URI}
    scrape_configs:
      - job_name: dsk-mongo-agent
        url: localhost:19216
        filtering_configs:
          rule: drop
EOF
```

| Argument      | Description              |
|---------------|--------------------------|
| --collect-all | 모든 종류의 메트릭을 수집하도록 설정합니다. |
| --mongodb.uri | mongodb의 주소를 설정합니다.      |

## 2. Mongo agent 설치

1. 데이터세이커가 사용할 로컬 디렉터리를 생성합니다.

   ```shell
    sudo mkdir -p /var/datasaker
    sudo chown -R datasaker:datasaker /var/datasaker/ 
   ```

2. 도커 명령어를 서버에 입력합니다.

   ```shell
    docker run -d --name dsk-mongo-agent\
    -v /var/datasaker/:/var/datasaker/\
    -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
    -v ~/.datasaker/mongo-config.yml:/etc/datasaker/agent-config.yml:ro\
    -e DSK_LOG_LEVEL=Info\
    --restart=always\
    datasaker/dsk-mongo-agent
   ```
