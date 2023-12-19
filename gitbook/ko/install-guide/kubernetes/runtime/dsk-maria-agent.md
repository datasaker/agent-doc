# dsk-maria-agent

MariaDB Agent는 MariaDBL의 모니터링 및 관리를 위한 도구입니다.
데이터베이스 운영 및 성능 향상을 위한 모니터링 기능을 제공합니다.
이 문서에는 MariaDB Agent의 설치, 설정에 대해 설명합니다.

## Support Version
| version | support |
|---------|:-------:|
| ver >= 10.5 | O |

## DataSaker 선행 작업을 진행하였나요?
현재 환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 \
`DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. \
[DataSaker 선행작업](README.md)

## MariaDB Agent 설치하기
### 1. MariaDB 설정 변경
관제하려는 데이터베이스의 `performance_schema`활성화가 필요합니다.
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

### 3. MariaDB Agent 설정값 등록
#### values.yaml
```yaml
mariaAgent:
  name:
  imgPolicy:
  imgVersion:
  logLevel:
  tolerations: []
  instances: []
  resources: {}
```
#### config.yaml 작성
```shell
cat << EOF >> ~/datasaker/config.yaml

mariaAgent:
  name: my-maria
  imgPolicy: 'Always'
  imgVersion: 'latest'
  logLevel: 'INFO'
  tolerations: []
  instances:
  - username: "datasaker"
    password: <your password>
    host: <db instance ip or domain>
    port: <db instance port>
    dbname: <db schema name>
    sslSkipVerfication: <certificate verification on the server during SSL connection>
    sslCa: <Path to the CA file>
    sslCert: <SSL certificate file path>
    sslKey: <SSL key file path>
    tls: <TLS Settings>
    appendSession:
      scrapeInterval: <append session collection cycle>
      dbList:
      - dbname: <db schema name>
        longSessionStandard: <long session standard>
```
##### **mariaAgent.instances**
| **Entity** | **Description** | **Default** | **Required** |
|------------|-----------------|-------------|--------------|
| username | 데이터베이스의 접속 권한을 가진 계정 정보 | N/A | **✓** |
| password | 데이터베이스의 접속 권한을 가진 계정 비밀전호 | N/A | **✓** |
| host | 데이터베이스의 주소 | N/A | **✓** |
| port | 데이터베이스의 포트번호 | N/A | **✓** |
| dbname | 데이터베이스의 schema 이름 | N/A | **✓** |
| sslSkipVerfication | SSL 연결 시 서버의 인증서 검증 유무 | false | N/A |
| sslCa | CA 파일의 경로 | N/A | N/A |
| sslCert | SSL 인증서 파일 경로 | N/A | N/A |
| sslKey | SSL 키 파일 경로 | N/A | N/A |
| tls | TLS 설정 | N/A | N/A |

##### mariaAgent.instances.appendSession
| **Entity** | **Description** | **Default** | **Required** |
|------------|-----------------|-------------|--------------|
| scrapeInterval | append session 수집 주기 | N/A | N/A |

##### mariaAgent.instance.appendSession.dbList
| **Entity** | **Description** | **Default** | **Required** |
|------------|-----------------|-------------|--------------|
| dbname | 데이터베이스의 schema 이름 | N/A | N/A |
| longSessionStandard | long session 기준 시간 | N/A | N/A |

### 4. MariaDB Agent 활성화
```
helm upgrade datasaker datasaker/agent-helm -n datasaker \ -f ~/datasaker/config.yaml
```