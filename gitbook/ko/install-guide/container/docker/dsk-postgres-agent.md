# dsk-postgres-agent

Postgres Agent는 PostgreSQL의 모니터링 및 관리를 위한 도구입니다.
데이터베이스 운영 및 성능 향상을 위한 모니터링 기능을 제공합니다.
이 문서에서는 Postgres Agent의 설치, 설정에 대해 설명합니다.

## Supported version
| version | support |
|---------|:-------:|
| ver >= 13 | O |

## DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 \
`DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. \
[DataSaker 선행 작업](README.md)

## Postgres Agent 설치하기
### 1. Postgres 설정 변경
관제하려는 데이터베이스의 `pg_stat_statements` 활성화가 필요합니다.
#### 활성화 확인
```
postgres=# show shared_preload_libraries;

 shared_preload_libraries
--------------------------
 pg_stat_statements
(1 row)
```
shared_preload_libraries에 `pg_stat_statements` 활성화가 필요합니다.

#### 필수 설정
| Parameter | Value | Description |
|-----------|-------|-------------|
| shared_preload_libraries | pg_stat_statements | `pg_stat_statements` extension을 이용하여 `pg_dsk_sql_stat.*` metric을 추출. |
| track_activity_query_size | 4096 | `pg_stat_activity`, `pg_stat_statements`의 SQL 텍스트 길이를 증가. |

#### 선택 설정
| Parameter | Value | Description |
|-----------|-------|-------------|
| pg_stat_statements.track | ALL | `stored producers`, `function` 내에서의 구문을 추척. |
| pg_stat_statements.max | 10000 | `pg_stat_statements`의 추적 쿼리 증가 |
| pg_stat_statements.track_utility | 0 | `PREPARE`, `EXPLAIN`같은 유틸리티 명령에 대한 추적을 비활성화 |

### 2. PostgreSQL User 권한 설정
`datasaker` 사용자를 생성하고 `pg_monitor` 및 기본 권한을 부여합니다.
```shell
CREATE USER datasaker WITH password '<PASSWORD>';
GRANT USAGE ON SCHEMA '<monitoring schema list>' TO datasaker
GRANT pg_monitor TO datasaker;
```

### 3. Postgres Agent 설정값 등록
#### postgres-config.yml
```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/postgres-config.yml

agent:
  metadata:
    agent_name: <agent alias>
    cluster_id: <cluster name>
  instances:
  - username: <db account name>
    password: <db account pass>
    host: <db instance ip or domain>
    port: <db instance port>
    dbname: <db schema name>
    sslmode: <certificate verification on the server during SSL connection>
    ssl-ca: <Path to the CA file>
    ssl-cert: <SSL certificate file path>
    ssl-key: <SSL key file path>
    append_session:
      scrape_interval: <append session collection cycle>
      db_list:
      - dbname: <db schema name>
        long_session_standard: <long session standard>
```
##### **metadata**
| **Settings** | **Description** | **Default** | **Required** |
|--------------|-----------------|:-----------:|:------------:|
| agent_name   | 에이전트 이름(별칭) | dsk-postgres-agent | N/A |
| cluster_id   | 관제 대상이 되는 환경이 어떤 클러스터로 묶여있는지에 대한 설정 | unknown | N/A |

##### **instances**
| **Settings** | **Description** | **Default** | **Required** |
|--------------|-----------------|:-----------:|:------------:|
| username     | 데이터베이스의 접속 권한을 가진 계정 정보 | N/A | **✓** |
| password     | 데이터베이스의 접속 권한을 가진 계정 비밀번호 | N/A | **✓** |
| host         | 데이터베이스의 주소 | N/A | **✓** |
| port         | 데이터베이스의 포트번호 | N/A | **✓** |
| sslmode      | SSL 활성화 유무 | disable | N/A |
| ssl-ca       | CA 파일의 경로 | N/A | N/A |
| ssl-cert     | SSL 인증서 파일 경로 | N/A | N/A |
| ssl-key      | SSL 키 파일 경로 | N/A | N/A |
| dbname       | 데이터베이스의 schema 이름 | N/A | **✓** |

##### **instances.append_session**
| **Settings** | **Description** | **Default** | **Required** |
|--------------|-----------------|:-----------:|:------------:|
| scrape_interval | append session 수집 주기 | N/A | N/A |

##### **instances.apped_session.db_list**
| **Settings** | **Description** | **Default** | **Required** |
|--------------|-----------------|:-----------:|:------------:|
| dbname       | 데이터베이스의 schema 이름 | N/A | N/A |
| long_session_standard | long session 기준 시간 | N/A | N/A |

##### example
* One DB Instance
	```yaml
	agent:
	  metadata:
	    agent_name: dsk-postgres-agent
	    cluster_id: example-cluster
	  instances:
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.1
	    port: 5432
	    dbname: example
	```
* One DB Instance, Append Session
	```yaml
	agent:
	  metadata:
	    agent_name: dsk-postgres-agent
	    cluster_id: example-cluster
	  instances:
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.1
	    port: 5432
	    dbname: example
	    append_session:
	      scrape_interval: 3s
	      db_list:
	      - dbname: append1
	        long_session_standard: 1s
	```

* Many DB Instance
	```yaml
	agent:
	  metadata:
	    agent_name: dsk-postgres-agent
	    cluster_id: example-cluster
	  instances:
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.1
	    port: 5432
	    dbname: example
	    append_session:
	      scrape_interval: 3s
	      db_list:
	      - dbname: append1
	        long_session_standard: 1s
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.2
	    port: 5432
	    dbname: example
	    append_session:
	      scrape_interval: 3s
	      db_list:
	      - dbname: append1
	        long_session_standard: 1s
	      - dbname: append2
	        long_session_standard: 3s
	```

### 4. Postgres Agent 설치
1. 데이터세이커가 사용할 로컬 디렉토리를 생성합니다.
    ```shell
    mkdir -p /var/datasaker
    chown -R datasaker:datasaker /var/datasaker/
    ```
2. 도커 명령어를 서버에 입력합니다.
    ```shell
    docker run -d --name dsk-postgres-agent \
    -v /var/datasaker/:/var/datasaker/ \
    -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro \
    -v ~/.datasaker/postgres-config.yml:/etc/datasaker/dsk-postgres-agent/agent-config.yml:ro \
    -e DSK_LOG_LEVEL=INFO \
    -- restart=always \
    datasaker/dsk-postgres-agent
    ```