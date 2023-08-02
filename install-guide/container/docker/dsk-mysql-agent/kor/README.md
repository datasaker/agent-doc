# Coming Soon
<!--
# 도커 환경에 DataSaker MySQL agent 설치하기

`MySQL agent`는 DataSaker에서 MySQL의 정보를 수집하는 agent입니다. 

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

# MySQL agent 설치하기

## 1. MySQL agent 설정값 등록

터미널에 다음 명령어를 입력하여 dsk-mysql-agent 설정 파일을 생성합니다.

```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/mysql-config.yml
agent:
  metadata:
   agent_name: dsk-mysql-agent
  option:
    exporter_config:
      command: "/etc/datasaker/target-exporter"
    scrape_configs:
      - job_name: dsk-mysql-agent
        metrics_path: /metrics/short
        url: localhost:19104
        filtering_configs:
          rule: drop
      - job_name: dsk-mysql-agent-long
        scrape_interval: 60s
        scrape_timeout: 10s
        metrics_path: /metrics/long
        url: localhost:19104
        filtering_configs:
          rule: drop
EOF
```

## 2. MySQL agent 설치

1. 데이터세이커가 사용할 로컬 디렉터리를 생성합니다.

   ```shell
    sudo mkdir -p /var/datasaker
    sudo chown -R datasaker:datasaker /var/datasaker/ 
   ```

2. MySQL 주소를 설정합니다.

에이전트를 연결하기 위해서는 수집하고자 하는 MySQL 서버의 호스트, 포트 주소를 에이전트에 설정해야 합니다.

   ```shell
    DSK_MYSQL_URI=http(s)://<user>:<password>@<host>:<port>
   ```

예를 들어, 주소가 `192.168.123.132`이고, `3306` 포트에 서비스중인 MySQL 서버를 수집하기 위해서는 다음과 같이 설정합니다.

   ```shell
    DSK_MYSQL_URI=http://192.168.123.132:3306
   ```

3. 도커 명령어를 서버에 입력합니다.

   ```shell
    docker run -d --name dsk-mysql-agent\
    -v /var/datasaker/:/var/datasaker/\
    -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
    -v ~/.datasaker/mysql-config.yml:/etc/datasaker/dsk-mysql-agent/agent-config.yml:ro\
    -e DATA_SOURCE_NAME=${DSK_MYSQL_URI}\
    -e DSK_LOG_LEVEL=INFO\
    -e DSK_SUB_KIND=mysql\
    --restart=always\
    datasaker/dsk-mysql-agent
   ```
-->