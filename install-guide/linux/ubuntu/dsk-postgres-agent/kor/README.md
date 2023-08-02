# Ubuntu 환경에 DataSaker Postgres agent 설치하기 (Beta)
`postgres agent`는 데이터베이스의 상태를 실시간으로 수집합니다.\
이를 통해 데이터베이스의 성능 지표, 리소스 사용량 등 다양한 정보를 수집할 수 있습니다.\
수집된 데이터를 기반으로 데이터베이스의 성능 병목 현상을 파악하고, 대응할 수 있습니다.\
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.
<br><br>

# Supported version
|version|support|
|---|---|
|postgres 15|X|
|postgres 14|O|
|postgres 13|X|
|postgres 12|X|
|postgres 11|X|
|postgres 10|X|
|postgres 9|X|
|postgres 8|X|

<br><br>

# DataSaker 선행 작업을 진행하였나요?
현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)
<br><br>

# Postgres agent 설치하기
## 1. Postgres 설정 변경
관제하려는 데이터베이스 `pg_stat_statements` 모듈의 활성화 된 상태인지 확인 부탁드립니다.\
[pg_stat_statements 참조사이트](https://www.postgresql.org/docs/14/pgstatstatements.html)

## 2. Postgres User 권한 설정
`postgres agent`를 설치하기 위해서는 `postgres user`의 권한이 필요합니다.\
`postgres user`의 권한을 확인하고, 권한이 없다면 권한을 부여해주세요.\
필요한 User 권한은 다음과 같습니다.
- `SELECT`
- `UPDATE`
- `DELETE`
- `INSERT`

[postgres user 권한 참조사이트](https://www.postgresql.org/docs/14/sql-grant.html)

## 3. 패키지 설치
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres-agent
```

## 4. agent-config 설정
`/etc/datasaker/dsk-postgres-agent/agent-config.yml`에 내용을 기입합니다.
```yaml
agent:
  metadata:
    agent_name: dsk-postgres-agent                # 에이전트 이름 (별칭) default=dsk-postgres-agent
  option:
    exporter_config:
      command: "/usr/bin/dsk-postgres-exporter"
      port: 19187
      args:
        - --extend.query-path=/etc/datasaker/dsk-postgres-agent/queries.yaml
        - --data-source-user=<monitoring account name>
        - --data-source-pass=<monitoring account pass>
        - --data-source-uri=<monitoring database uri> # <ip>:<port>/dbname
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-postgres-agent
        url: localhost:19187
        filtering_configs:
          rule: drop
```

## 5. 패키지 실행
```shell
systemctl enable dsk-postgres-agent --now
```

## 6. 패키지 실행 상태 확인
```shell
systemctl status dsk-postgres-agent
```
또는
```shell
service dsk-postgres-agent
```

# Postgres agent 제거하기

## 1. 패키지 중단
```shell
systemctl stop dsk-postgres-agent
```

## 2. 패키지 제거
```shell
sudo apt remove dsk-postgres-agent
```
