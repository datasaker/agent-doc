# dsk-postgres-agent

Postgres Agent는 PostgreSQL의 모니터링 및 관리를 위한 도구입니다.
데이터베이스 운영 및 성능 향상을 위한 모니터링 기능을 제공합니다.
이 문서에서는 Postgres Agent의 설치, 설정에 대해 설명합니다.

## Supported version
| version | support |
|---------|:-------:|
| ver >= 13 | O |

## DataSaker 선행 작업을 진행하였나요?
현재 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 \
`DataSaker`선행 작업을 먼저 진행하여 주시기 바랍니다. \
[DataSaker 선행작업](README.md)

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
#### values.yaml
```yaml
postgresAgent:
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

postgresAgent:
  name: my-postgres
  imgPolicy: 'Always'
  imgVersion: 'latest'
  logLevel 'INFO'
  tolerations: []
  instances:
  - username: "datasaker"
    password: <your password>
    host: <db instance ip or domain>
    port: <db instance port>
    dbname: <db schema name>
    sslMode: <certificate verification on the server during SSL connection>
    sslCa: <Path to the CA file>
    sslCert: <SSL certificate file path>
    sslKey: <SSL key file path>
    appendSession:
      scrapeInterval: <append session collection cycle>
      dbList:
      - dbname: <db schema name>
        longSessionStandard: <long session standard>
```
##### **postgresAgent.instances**
| **Entity** | **Description** | **Default** | **Required** |
|------------|-----------------|-------------|--------------|
| username | 데이터베이스의 접속 권한을 가진 계정 정보 | N/A | **✓** |
| password | 데이터베이스의 접속 권한을 가진 계정 비밀전호 | N/A | **✓** |
| host | 데이터베이스의 주소 | N/A | **✓** |
| port | 데이터베이스의 포트번호 | N/A | **✓** |
| dbname | 데이터베이스의 schema 이름 | N/A | **✓** |
| sslMode | SSL 연결 시 서버의 인증서 검증 유무 | false | N/A |
| sslCa | CA 파일의 경로 | N/A | N/A |
| sslCert | SSL 인증서 파일 경로 | N/A | N/A |
| sslKey | SSL 키 파일 경로 | N/A | N/A |

##### postgresAgent.instances.appendSession
| **Entity** | **Description** | **Default** | **Required** |
|------------|-----------------|-------------|--------------|
| scrapeInterval | append session 수집 주기 | N/A | N/A |

##### postgresAgent.instance.appendSession.dbList
| **Entity** | **Description** | **Default** | **Required** |
|------------|-----------------|-------------|--------------|
| dbname | 데이터베이스의 schema 이름 | N/A | N/A |
| longSessionStandard | long session 기준 시간 | N/A | N/A |

### 4. Postgres Agent 활성화
```
helm upgrade datasaker datasaker/agent-helm -n datasaker \ -f ~/datasaker/config.yaml
```