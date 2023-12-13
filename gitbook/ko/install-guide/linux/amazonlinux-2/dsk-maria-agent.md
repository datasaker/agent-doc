# dsk-maria-agent

MariaDB Agent는 MariaDB의 모니터링 및 관리를 위한 도구입니다.
데이터베이스 운영 및 성능 향상을 위한 모니터링 기능을 제공합니다.
이 문서에서는 MariaDB Agent의 설치, 설정에 대해 설명합니다.

## Support Version
| version | support |
|---------|:-------:|
| ver >= 10.5 | O |

## DataSaker 선행 작업을 진행하였나요?
현재 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 \
`DataSaker`선행 작업을 먼저 진행하여 주시기 바랍니다. \
[DataSaker 선행작업](README.md)

## MariaDB Agent 설치하기
### 1. MariaDB 설정 변경
관제하려는 데이터베이스의 `performance_schema` 활성화가 필요합니다.
#### 활성화 확인
```shell
MariaDB > SHOW GLOBAL VARIABLES LIKE 'performance_schema';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| performance_schema | ON    |
+--------------------+-------+
```
Value 값이 OFF라면 `performance_schema` 활성화가 필요합니다.

##### Dynamic
* setup_instruments update
	```shell
	-- 활성화
	UPDATE performance_schema.setup_instruments SET enabled='YES', timed='YES' WHERE name='statement/sql/select';

	UPDATE performance_schema.setup_consumers SET enabled='YES' WHERE name LIKE 'events_%current';
	UPDATE performance_schema.setup_consumers SET enabled='YES' WHERE name = 'statements_digest';
	UPDATE performance_schema.setup_consumers SET enabled='YES' WHERE name = 'thread_instrumentation';
	```
	테이블을 업데이트하는 방식은 다이나믹한 방식으로 영구적이지 않습니다.\
	MariaDB 서버를 재시작 하는 경우 다시 적용해야합니다.

##### Static
* performance_schema.cnf
	```
	[mysqld]
	performance_schema=ON
	performance_schema_consumer_events_stages_current=ON
	performance_schema_consumer_events_statements_current=ON
	performance_schema_consumer_events_transactions_current=ON
	performance_schema_consumer_events_waits_current=ON
	performance_schema_consumer_statements_digest=ON
	performance_schema_consumer_thread_instrumentation=ON
	```
	cnf를 적용하려면 데이터베이스를 재기동 해야하니 주의가 필요합니다.

### 2. MariaDB User 권한 설정
`datasaker` 사용자를 생성하고 기본 권한을 부여합니다.
```shell
CREATE USER 'datasaker'@'%' IDENTIFIED by '<PASSWORD>';
GRANT SELECT ON performance_schema.* TO 'datasaker'@'%';
GRANT PROCESS, SLAVE MONITOR ON *.* TO 'datasaker'@'%';
```

### 3. 패키지 설치
```shell
yum install dsk-maria-agent
```

### 4. agent-config 설정
```shell
vi /etc/datasaker/dsk-maria-agent/agent-config.yml
```

#### agent-config.yml
```yaml
agent:
  metadata:
    agent_name:
    cluster_id:
  instances:
  - username:
    password:
    host:
    port:
    ssl-skip-verfication:
    ssl-ca:
    ssl-cert:
    ssl-key:
    tls:
    dbname:
    append_session:
      scrape_interval:
      db_list:
      - dbname:
        long_session_standard:
```
##### **metadata**
| **Settings** | **Description** | **Default** | **Required** |
|--------------|-----------------|:-----------:|:------------:|
| agent_name   | 에이전트 이름(별칭) | dsk-maria-agent | N/A |
| cluster_id   | 관제 대상이 되는 환경이 어떤 클러스터로 묶여있는지에 대한 설정 | unknown | N/A |

##### **instances**
| **Settings** | **Description** | **Default** | **Required** |
|--------------|-----------------|:-----------:|:------------:|
| username     | 데이터베이스의 접속 권한을 가진 계정 정보 | N/A | **✓** |
| password     | 데이터베이스의 접속 권한을 가진 계정 비밀번호 | N/A | **✓** |
| host         | 데이터베이스의 주소 | N/A | **✓** |
| port         | 데이터베이스의 포트번호 | N/A | **✓** |
| ssl-skip-verfication | SSL 연결 시 서버의 인증서 검증 유무 | false | N/A |
| ssl-ca       | CA 파일의 경로 | N/A | N/A |
| ssl-cert     | SSL 인증서 파일 경로 | N/A | N/A |
| ssl-key      | SSL 키 파일 경로 | N/A | N/A |
| tls          | TLS 설정 | N/A | N/A |
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
	    agent_name: dsk-maria-agent
	    cluster_id: example-cluster
	  instances:
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.1
	    port: 3306
	    ssl-skip-verfication: true
	    dbname: example
	```
* One DB Instance, Append Session
	```yaml
	agent:
	  metadata:
	    agent_name: dsk-maria-agent
	    cluster_id: example-cluster
	  instances:
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.1
	    port: 3306
	    ssl-skip-verfication: true
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
	    agent_name: dsk-maria-agent
	    cluster_id: example-cluster
	  instances:
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.1
	    port: 3306
	    ssl-skip-verfication: true
	    dbname: example
	    append_session:
	      scrape_interval: 3s
	      db_list:
	      - dbname: append1
	        long_session_standard: 1s
	  - username: datasaker
	    password: <account password>
	    host: 127.0.0.2
	    port: 3307
	    ssl-skip-verfication: true
	    dbname: example
	    append_session:
	      scrape_interval: 3s
	      db_list:
	      - dbname: append1
	        long_session_standard: 1s
	      - dbname: append2
	        long_session_standard: 3s
	```

### 5. 패키지 실행
```shell
systemctl enable dsk-maria-agent --now
```

### 6. 패키지 실행 상태 확인
```shell
systemctl status dsk-maria-agent
```

## MariaDB Agent 제거하기
### 1. 패키지 중단
```shell
systemctl stop dsk-maria-agent
```

### 2. 패키지 제거
```shell
yum remove dsk-maria-agent
```