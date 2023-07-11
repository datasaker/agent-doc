# Ansible Datasaker Role

Ansible을 이용하여 Datasaker Agent를 설치할 수 있습니다.

## Requirements

* Ansible v2.6+가 필요합니다.
* 대부분의 데비안 리눅스 배포판을 지원합니다.
* Amazon Linux 2 배포판을 지원합니다.

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

| 변수명                                          | 설명                                                                                                                                                                                                                                                                                                                  |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `datasaker_api_key`                          | API Key를 입력합니다.                                                                                                                                                                                                                                                                                                     |
| `datasaker_agents`                           | <p>각 호스트에 설치하고자 하는 Host Agent 리스트입니다.<br><code>dsk-node-agent</code> <code>dsk-trace-agent</code> <code>dsk-log-agent</code> <code>dsk-postgres-agent</code> <code>dsk-plan-postgres-agent</code><br>(Default) <code>dsk-node-agent</code></p>                                                                      |
| `datasaker_docker_agents`                    | <p>각 호스트에 설치하고자 하는 Docker Container Agent 리스트입니다. Docker Container Agents를 넣으면 Host Agent 설치는 자동으로 비활성화 됩니다.<br><code>dsk-docker-node-agent</code> <code>dsk-docker-trace-agent</code> <code>dsk-docker-log-agent</code> <code>dsk-docker-postgres-agent</code><br>(Default) <code>dsk-docker-node-agent</code></p> |
| \<!--                                        |                                                                                                                                                                                                                                                                                                                     |
| #### Datasaker 공통 설정                         |                                                                                                                                                                                                                                                                                                                     |
| 변수명                                          | 설명                                                                                                                                                                                                                                                                                                                  |
| -------------------------------------------- | --------------------------------------------------                                                                                                                                                                                                                                                                  |
| `datagate_url`                               | <p>Datasaker Agent가 전송하는 Datasaker Datagate URL 설정.<br>(Default) <code>gate.kr.datasaker.io</code></p>                                                                                                                                                                                                              |
| `datagate_trace_url`                         | <p><code>dsk-trace-agent</code> Datagate URL 설정.<br>(Default) <code>datagate_url</code></p>                                                                                                                                                                                                                         |
| `datagate_trace_port`                        | <p><code>dsk-trace-agent</code> Datagate Port 설정.<br>(Default) <code>31300</code></p>                                                                                                                                                                                                                               |
| `datagate_trace_timeout`                     | <p><code>dsk-trace-agent</code> Data 전송 만료 시간 설정.<br>(Default) <code>5s</code></p>                                                                                                                                                                                                                                  |
| `datagate_manifest_url`                      | <p><code>dsk-manifest-agent</code> Datagate URL 설정.<br>(Default) <code>datagate_url</code></p>                                                                                                                                                                                                                      |
| `datagate_manifest_port`                     | <p><code>dsk-manifest-agent</code> Datagate Port 설정.<br>(Default) <code>31301</code></p>                                                                                                                                                                                                                            |
| `datagate_manifest_timeout`                  | <p><code>dsk-manifest-agent</code> Data 전송 만료 시간 설정.<br>(Default) <code>5s</code></p>                                                                                                                                                                                                                               |
| `datagate_metric_url`                        | <p><code>dsk-metric-agent</code> Datagate URL 설정.<br>(Default) <code>datagate_url</code></p>                                                                                                                                                                                                                        |
| `datagate_metric_port`                       | <p><code>dsk-metric-agent</code> Datagate Port 설정.<br>(Default) <code>31302</code></p>                                                                                                                                                                                                                              |
| `datagate_metric_timeout`                    | <p><code>dsk-metric-agent</code> Data 전송 만료 시간 설정.<br>(Default) <code>5s</code></p>                                                                                                                                                                                                                                 |
| `datagate_plan_url`                          | <p><code>dsk-plan-agent</code> Datagate URL 설정.<br>(Default) <code>datagate_url</code></p>                                                                                                                                                                                                                          |
| `datagate_plan_port`                         | <p><code>dsk-plan-agent</code> Datagate Port 설정.<br>(Default) <code>31303</code></p>                                                                                                                                                                                                                                |
| `datagate_plan_timeout`                      | <p><code>dsk-plan-agent</code> Data 전송 만료 시간 설정.<br>(Default) <code>5s</code></p>                                                                                                                                                                                                                                   |
| `datagate_loggate_url`                       | <p><code>dsk-log-agent</code> Datagate URL 설정.<br>(Default) <code>datagate_url</code></p>                                                                                                                                                                                                                           |
| `datagate_loggate_port`                      | <p><code>dsk-log-agent</code> Datagate Port 설정.<br>(Default) <code>31304</code></p>                                                                                                                                                                                                                                 |
| `datagate_loggate_timeout`                   | <p><code>dsk-log-agent</code> Data 전송 만료 시간 설정.<br>(Default) <code>5s</code></p>                                                                                                                                                                                                                                    |
| `datasaker_api_url`                          | <p>Datasaker API Server URL.<br>(Default) <code>api.kr.datasaker.io</code></p>                                                                                                                                                                                                                                      |
| `datasaker_api_send_interval`                | <p>Datasaker API Server Data 전송 만료 시간 설정.<br>(Default) <code>1m</code></p>                                                                                                                                                                                                                                          |
| -->                                          |                                                                                                                                                                                                                                                                                                                     |

#### Docker Container Agent 설정

| 변수명                                  | 설명                                                                                                                 |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| `datasaker_docker_config_path`       | <p>Datasaker Global Config 위치 설정.<br>(Default) <code>~/.datasaker</code></p>                                       |
| `datasaker_docker_global_config`     | <p>Datasaker Global Config 이름 설정.<br>(Default) <code>~/.datasaker/config.yml</code></p>                            |
| `docker_default_path`                | <p>Datasaker Docker Log Agent에 마운트할 Docker Log 수집 위치 설정.<br>(Default) <code>/var/lib/docker/containers/</code></p> |
| `datasaker_docker_path`              | <p>Datasaker Docker Agent Container 위치 설정.<br>(Default) <code>/var/datasaker</code></p>                            |
| `container_agent_restart_policy`     | <p><code>dsk-container-agent</code> Container Restart Policy 설정.<br>(Default) <code>always</code></p>              |
| `node_agent_restart_policy`          | <p><code>dsk-node-agent</code> Container Restart Policy 설정.<br>(Default) <code>always</code></p>                   |
| `trace_agent_restart_policy`         | <p><code>dsk-trace-agent</code> Container Restart Policy 설정.<br>(Default) <code>always</code></p>                  |
| `log_agent_restart_policy`           | <p><code>dsk-log-agent</code> Container Restart Policy 설정.<br>(Default) <code>always</code></p>                    |
| `postgres_agent_restart_policy`      | <p><code>dsk-postgres-agent</code> Container Restart Policy 설정.<br>(Default) <code>always</code></p>               |
| `plan_postgres_agent_restart_policy` | <p><code>dsk-plan-postgres-agent</code> Container Restart Policy 설정.<br>(Default) <code>always</code></p>          |
| `container_agent_log_level`          | <p><code>dsk-container-agent</code> Log Level 설정.<br>(Default) <code>INFO</code></p>                               |
| `node_agent_log_level`               | <p><code>dsk-node-agent</code> Log Level 설정.<br>(Default) <code>INFO</code></p>                                    |
| `trace_agent_log_level`              | <p><code>dsk-trace-agent</code> Log Level 설정.<br>(Default) <code>INFO</code></p>                                   |
| `log_agent_log_level`                | <p><code>dsk-log-agent</code> Log Level 설정.<br>(Default) <code>INFO</code></p>                                     |
| `postgres_agent_log_level`           | <p><code>dsk-postgres-agent</code> Log Level 설정.<br>(Default) <code>INFO</code></p>                                |
| `plan_postgres_agent_log_level`      | <p><code>dsk-plan-postgres-agent</code> Log Level 설정.<br>(Default) <code>INFO</code></p>                           |

#### Datasaker Agent 상세 설정

* Host Agent 와 Docker Container Agent는 같은 설정값을 사용합니다.

| 변수명                                   | 설명                                                                                                                                                                                         |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `trace_sampling_rate`                 | <p><code>dsk-trace-agent</code> 에서 collector에 적용되는 샘플링 비율 설정.<br>- 100 이상일 때 모든 데이터가 수집.<br>(Default) <code>1</code></p>                                                                   |
| `log_collects`                        | `dsk-log-agent` 에서 로그 수집 컬렉션 구성 설정. 리스트의 각 항목에는 아래 항목들이 포함.                                                                                                                                |
| `log_collects[*].paths`               | <p><code>dsk-log-agent</code> 에서 로그 수집을 위한 경로 설정.<br>(Default) [<code>host-agent</code>]=<code>/var/log/*.log</code>, [<code>docker-agent</code>]=<code>/var/log/sample/*/*.log</code></p> |
| `log_collects[*].exclude_paths`       | <p><code>dsk-log-agent</code> 에서 로그 컬렉션에서 제외할 경로 설정. 값을 지정하지 않으면 수집 경로에 설정한 모든 로그 수집.<br>(Default) <code>None</code></p>                                                                   |
| `log_collects[*].keywords`            | <p><code>dsk-log-agent</code> 에서 로그를 필터링하기 위한 키워드 설정. 값을 지정하지 않으면 모든 로그 수집.<br>(Default) <code>None</code></p>                                                                             |
| `log_collects[*].tag`                 | <p><code>dsk-log-agent</code> 에서 사용자 설정 태그.<br>(Default) <code>Default</code></p>                                                                                                          |
| `log_collects[*].service.name`        | <p><code>dsk-log-agent</code> 에서 수집하는 서비스명 설정.<br>(Default) <code>default</code></p>                                                                                                       |
| `log_collects[*].service.category`    | <p><code>dsk-log-agent</code> 에서 수집하는 서비스 분류값 설정.<br><code>app</code> <code>database</code> <code>syslog</code> <code>etc</code> (Default) <code>etc</code></p>                            |
| `log_collects[*].service.type`        | <p><code>dsk-log-agent</code> 에서 수집하는 서비스 타입 설정.<br><code>postgres</code> <code>mysql</code> <code>java</code> <code>etc</code> (Default) <code>etc</code></p>                             |
| `log_collects[*].service.address`     | <p><code>dsk-log-agent</code> 에서 수집하는 서비스 타입이 Database인 경우에 작성<br>- 데이터베이스 host 및 port 정보</p>                                                                                              |
| `postgres_user_name`                  | <p><code>dsk-postgres-agent</code>에 Postgres user ID 설정.<br>(Default) <code>None</code></p>                                                                                                |
| `postgres_user_password`              | <p><code>dsk-postgres-agent</code>에 Postgres user password 설정.<br>(Default) <code>None</code></p>                                                                                          |
| `postgres_database_address`           | <p><code>dsk-postgres-agent</code>에 Postgres address 설정.<br>(Default) <code>None</code></p>                                                                                                |
| `postgres_database_port`              | <p><code>dsk-postgres-agent</code>에 Postgres port 설정.<br>(Default) <code>None</code></p>                                                                                                   |
| `plan_postgres_user_name`             | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres user ID 설정.<br>(Default) <code>None</code></p>                                                                                      |
| `plan_postgres_user_password`         | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres user password 설정.<br>(Default) <code>None</code></p>                                                                                |
| `plan_postgres_database_address`      | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres address 설정.<br>(Default) <code>None</code></p>                                                                                      |
| `plan_postgres_database_port`         | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres port 설정.<br>(Default) <code>None</code></p>                                                                                         |
| `plan_postgres_database_name`         | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres database 설정.<br>(Default) <code>None</code></p>                                                                                     |
| `plan_postgres_scrape_interval`       | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres scrape interval 설정.<br>(Default) <code>30s</code></p>                                                                               |
| `plan_postgres_scrape_timeout`        | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres scrape timeout 설정.<br>(Default) <code>5s</code></p>                                                                                 |
| `plan_postgres_slow_query_standard`   | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres slow query standard 설정.<br>(Default) <code>5s</code></p>                                                                            |
| `plan_postgres_executor_number`       | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres executor number 설정.<br>(Default) <code>10</code></p>                                                                                |
| `plan_postgres_sender_number`         | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres sender number 설정.<br>(Default) <code>10</code></p>                                                                                  |
| `plan_postgres_activity_query_buffer` | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres activity query buffer 설정.<br>(Default) <code>50</code></p>                                                                          |
| `plan_postgres_plan_sender_buffer`    | <p><code>dsk-plan-postgres-agent</code>에 Plan Postgres plan sender buffer 설정.<br>(Default) <code>50</code></p>                                                                             |

**Ansible Playbook 상세 설정 Example**

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

Datasaker Agent를 제거 할 수 있습니다. datasaker\_clean은 uninstall이 `True`로 설정되어야 합니다.

| 변수명               | 설명                                                                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `uninstall`       | <p><code>datasaker_agents</code> 또는 <code>datasaker_docker_agents</code> 에 작성된 Agent만 제거.<br>(Default) <code>False</code></p>                    |
| `datasaker_clean` | <p><code>datasaker_agents</code> 또는 <code>datasaker_docker_agents</code> 에 작성된 Agent 와 생성 된 폴더 및 설정 파일까지 제거.<br>(Default) <code>False</code></p> |

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
