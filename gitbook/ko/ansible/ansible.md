# Ansible Datasaker Role

Ansible을 이용하여 Datasaker Agent를 설치할 수 있습니다.

## Requirements

- Ansible v2.6+가 필요합니다.
- 대부분의 데비안 리눅스 배포판을 지원합니다.
- Amazon Linux 2 배포판을 지원합니다.

## Installation

Ansible Galaxy에서 Datasaker role을 설치합니다.

```shell
ansible-galaxy install dsk_bot.datasaker
```

에이전트를 배포하기 위하여 Ansible playbook을 작성합니다.

아래는 기본 설치에 대한 예시입니다.

#### Host Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent"] 
```
#### Docker Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents: ["dsk-docker-node-agent","dsk-docker-log-agent"]
```

#### 필수 설정

| 변수명                                      | 설명                                      |
|--------------------------------------------|--------------------------------------------------|
|`datasaker_api_key`|API Key를 입력합니다.|
|`datasaker_agents`| 각 호스트에 설치하고자 하는 Host Agent 리스트입니다. <br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>(Default) `dsk-node-agent`|
|`datasaker_docker_agents`| 각 호스트에 설치하고자 하는 Docker Container Agent 리스트입니다. Docker Container Agents를 넣으면 Host Agent 설치는 자동으로 비활성화 됩니다. <br>`dsk-docker-node-agent` `dsk-docker-trace-agent` `dsk-docker-log-agent` `dsk-docker-postgres-agent`<br>(Default) `dsk-docker-node-agent`|

<!--
#### Datasaker 공통 설정
| 변수명                                      | 설명                                      |
|--------------------------------------------|--------------------------------------------------|
|`datagate_url`| Datasaker Agent가 전송하는 Datasaker Datagate URL 설정. <br>(Default) `gate.kr.datasaker.io`|
|`datagate_trace_url`| `dsk-trace-agent` Datagate URL 설정. <br>(Default) `datagate_url`|
|`datagate_trace_port`| `dsk-trace-agent` Datagate Port 설정. <br>(Default) `31300`|
|`datagate_trace_timeout`| `dsk-trace-agent` Data 전송 만료 시간 설정. <br>(Default) `5s`|
|`datagate_manifest_url`| `dsk-manifest-agent` Datagate URL 설정. <br>(Default) `datagate_url`|
|`datagate_manifest_port`| `dsk-manifest-agent` Datagate Port 설정. <br>(Default) `31301`|
|`datagate_manifest_timeout`| `dsk-manifest-agent` Data 전송 만료 시간 설정. <br>(Default) `5s`|
|`datagate_metric_url`| `dsk-metric-agent` Datagate URL 설정. <br>(Default) `datagate_url`|
|`datagate_metric_port`| `dsk-metric-agent` Datagate Port 설정. <br>(Default) `31302`|
|`datagate_metric_timeout`| `dsk-metric-agent` Data 전송 만료 시간 설정. <br>(Default) `5s`|
|`datagate_plan_url`| `dsk-plan-agent` Datagate URL 설정. <br>(Default) `datagate_url`|
|`datagate_plan_port`| `dsk-plan-agent` Datagate Port 설정. <br>(Default) `31303`|
|`datagate_plan_timeout`| `dsk-plan-agent` Data 전송 만료 시간 설정. <br>(Default) `5s`|
|`datagate_loggate_url`| `dsk-log-agent` Datagate URL 설정. <br>(Default) `datagate_url`|
|`datagate_loggate_port`| `dsk-log-agent` Datagate Port 설정. <br>(Default) `31304`|
|`datagate_loggate_timeout`| `dsk-log-agent` Data 전송 만료 시간 설정. <br>(Default) `5s`|
|`datasaker_api_url`| Datasaker API Server URL. <br>(Default) `api.kr.datasaker.io`|
|`datasaker_api_send_interval`| Datasaker API Server Data 전송 만료 시간 설정. <br>(Default) `1m`|
-->

#### Docker Container Agent 설정
| 변수명                                      | 설명                                      |
|--------------------------------------------|--------------------------------------------------|
|`datasaker_docker_config_path`| Datasaker Global Config 위치 설정. <br> (Default) `~/.datasaker`|
|`datasaker_docker_global_config`| Datasaker Global Config 이름 설정. <br> (Default) `~/.datasaker/config.yml`|
|`docker_default_path`| Datasaker Docker Log Agent에 마운트할 Docker Log 수집 위치 설정. <br> (Default) `/var/lib/docker/containers/`|
|`datasaker_docker_path`| Datasaker Docker Agent Container 위치 설정. <br> (Default) `/var/datasaker`|
|`container_agent_restart_policy`| `dsk-container-agent` Container Restart Policy 설정. <br> (Default) `always`|
|`node_agent_restart_policy`|  `dsk-node-agent` Container Restart Policy 설정. <br> (Default) `always`|
|`trace_agent_restart_policy`|  `dsk-trace-agent` Container Restart Policy 설정. <br> (Default) `always`|
|`log_agent_restart_policy`|  `dsk-log-agent` Container Restart Policy 설정. <br> (Default) `always`|
|`postgres_agent_restart_policy`|  `dsk-postgres-agent` Container Restart Policy 설정. <br> (Default) `always`|
|`plan_postgres_agent_restart_policy`|  `dsk-plan-postgres-agent` Container Restart Policy 설정. <br> (Default) `always`|
|`container_agent_log_level`| `dsk-container-agent` Log Level 설정. <br> (Default) `INFO`|
|`node_agent_log_level`| `dsk-node-agent` Log Level 설정. <br> (Default) `INFO`|
|`trace_agent_log_level`| `dsk-trace-agent` Log Level 설정. <br> (Default) `INFO`|
|`log_agent_log_level`| `dsk-log-agent` Log Level 설정. <br> (Default) `INFO`|
|`postgres_agent_log_level`| `dsk-postgres-agent` Log Level 설정. <br> (Default) `INFO`|
|`plan_postgres_agent_log_level`| `dsk-plan-postgres-agent` Log Level 설정. <br> (Default) `INFO`|

<!--|`datasaker_docker_user`| Datasaker Docker Container Directory Ownership 설정. <br> (Default) `datasaker`|
|`datasaker_docker_group`| Datasaker Docker Container Directory Group 설정. <br> (Default) `datasaker`|
|`datasaker_docker_user_uid`| Datasaker Docker Container Agent User UID 설정 <br> (Default) `202306`|
|`datasaker_docker_user_gid`| Datasaker Docker Container Agent User GID 설정 <br> (Default) `202306`|
|`container_agent_image_tag`| `dsk-container-agent` Image tag 설정. <br> (Default) `latest`|
|`node_agent_image_tag`| `dsk-node-agent` Image tag 설정. <br> (Default) `latest`|
|`trace_agent_image_tag`| `dsk-trace-agent` Image tag 설정. <br> (Default) `latest`|
|`log_agent_image_tag`| `dsk-log-agent` Image tag 설정. <br> (Default) `latest`|
|`postgres_agent_image_tag`| `dsk-postgres-agent` Image tag 설정. <br> (Default) `latest`|
|`plan_postgres_agent_image_tag`| `dsk-plan-postgres-agent` Image tag 설정. <br> (Default) `latest`|-->

#### Datasaker Agent 상세 설정
- Host Agent 와 Docker Container Agent는 같은 설정값을 사용합니다.

| 변수명                                      | 설명                                      |
|--------------------------------------------|--------------------------------------------------|
|`trace_sampling_rate`| `dsk-trace-agent` 에서 collector에 적용되는 샘플링 비율 설정.<br>- 100 이상일 때 모든 데이터가 수집.<br> (Default) `1`|
|`log_collects`|	`dsk-log-agent` 에서 로그 수집 컬렉션 구성 설정. 리스트의 각 항목에는 아래 항목들이 포함. |
|`log_collects[*].paths`|	`dsk-log-agent` 에서 로그 수집을 위한 경로 설정. <br> (Default) [`host-agent`]=`/var/log/*.log`, [`docker-agent`]=`/var/log/sample/*/*.log` |
|`log_collects[*].exclude_paths`|	`dsk-log-agent` 에서 로그 컬렉션에서 제외할 경로 설정. 값을 지정하지 않으면 수집 경로에 설정한 모든 로그 수집. <br> (Default) `None` |
|`log_collects[*].keywords`|	`dsk-log-agent` 에서 로그를 필터링하기 위한 키워드 설정. 값을 지정하지 않으면 모든 로그 수집. <br> (Default) `None` |
|`log_collects[*].tag`|	`dsk-log-agent` 에서 사용자 설정 태그. <br> (Default) `Default` |
|`log_collects[*].service.name`|	`dsk-log-agent` 에서 수집하는 서비스명 설정. <br> (Default) `default` |
|`log_collects[*].service.category`|	`dsk-log-agent` 에서 수집하는 서비스 분류값 설정. <br> `app` `database` `syslog` `etc` (Default) `etc` |
|`log_collects[*].service.type`|	`dsk-log-agent` 에서 수집하는 서비스 타입 설정. <br> `postgres` `mysql` `java` `etc` (Default) `etc` |
|`log_collects[*].service.address`|	`dsk-log-agent` 에서 수집하는 서비스 타입이 Database인 경우에 작성 <br>- 데이터베이스 host 및 port 정보 |
|`postgres_user_name`| `dsk-postgres-agent`에 Postgres user ID 설정. <br> (Default) `None` |
|`postgres_user_password`| `dsk-postgres-agent`에 Postgres user password 설정. <br> (Default) `None` |
|`postgres_database_address`| `dsk-postgres-agent`에 Postgres address 설정. <br> (Default) `None` |
|`postgres_database_port`| `dsk-postgres-agent`에 Postgres port 설정. <br> (Default) `None` |
|`plan_postgres_user_name`| `dsk-plan-postgres-agent`에 Plan Postgres user ID 설정. <br> (Default) `None` |
|`plan_postgres_user_password`| `dsk-plan-postgres-agent`에 Plan Postgres user password 설정. <br> (Default) `None` |
|`plan_postgres_database_address`| `dsk-plan-postgres-agent`에 Plan Postgres address 설정. <br> (Default) `None` |
|`plan_postgres_database_port`| `dsk-plan-postgres-agent`에 Plan Postgres port 설정. <br> (Default) `None` |
|`plan_postgres_database_name`| `dsk-plan-postgres-agent`에 Plan Postgres database 설정. <br> (Default) `None` |
|`plan_postgres_scrape_interval`| `dsk-plan-postgres-agent`에 Plan Postgres scrape interval 설정. <br> (Default) `30s` |
|`plan_postgres_scrape_timeout`| `dsk-plan-postgres-agent`에 Plan Postgres scrape timeout 설정. <br> (Default) `5s` |
|`plan_postgres_slow_query_standard`| `dsk-plan-postgres-agent`에 Plan Postgres slow query standard 설정. <br> (Default) `5s` |
|`plan_postgres_executor_number`| `dsk-plan-postgres-agent`에 Plan Postgres executor number 설정. <br> (Default) `10` |
|`plan_postgres_sender_number`| `dsk-plan-postgres-agent`에 Plan Postgres sender number 설정. <br> (Default) `10` |
|`plan_postgres_activity_query_buffer`| `dsk-plan-postgres-agent`에 Plan Postgres activity query buffer 설정. <br> (Default) `50` |
|`plan_postgres_plan_sender_buffer`| `dsk-plan-postgres-agent`에 Plan Postgres plan sender buffer 설정. <br> (Default) `50` |

##### Ansible Playbook 상세 설정 Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents:
      - "dsk-docker-node-agent"
      - "dsk-docker-trace-agent"
      - "dsk-docker-log-agent"
      - "dsk-docker-postgres-agent"
    postgres_user_name: sample
    postgres_user_password: 1q2w3e4r
    postgres_database_address: 0.0.0.0
    postgres_database_port: 5432
    plan_postgres_user_name: sample
    plan_postgres_user_password: 1q2w3e4r
    plan_postgres_database_address: 0.0.0.0
    plan_postgres_database_name: sample
    plan_postgres_database_port: 5432
    log_collects:
      - paths:
          - "/var/log/sample/*/*.log"
        exclude_paths: []
        keywords: []
        tag: "Default"
        service:
          name: "default"
          category: "etc"
          type: "etc"
      - paths:
          - "/var/log/sample/b4d5ac015a5a*/*.log"
        service:
          name: "docker-test"
          category: "database"
          type: "postgres"
          address: "0.0.0.0:5432"
```

## Uninstallation

Datasaker Agent를 제거 할 수 있습니다. 
datasaker_clean은 uninstall이 `True`로 설정되어야 합니다.

| 변수명                                      | 설명                                      |
|--------------------------------------------|--------------------------------------------------|
|`uninstall`| `datasaker_agents` 또는 `datasaker_docker_agents` 에 작성된 Agent만 제거. <br> (Default) `False`|
|`datasaker_clean`|	`datasaker_agents` 또는 `datasaker_docker_agents` 에 작성된 Agent 와 생성 된 폴더 및 설정 파일까지 제거.  <br> (Default) `False`|

#### Datasaker Agents Uninstall Example

```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_agents: ["<AGENT_NAME>"]
    uninstall: True
    datasaker_clean: True
```
<!--
## Troubleshooting

### Debian stretch

**Note:** 
-->
