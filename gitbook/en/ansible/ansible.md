# Ansible Datasaker Role

You can install Datasaker Agent using Ansible.

## Requirements

- Requires Ansible v2.6+.
- Supports most Debian Linux distributions.
- Supports Amazon Linux 2 distribution.

## Installation

Install the Datasaker role in Ansible Galaxy.

```shell
ansible-galaxy install dsk_bot.datasaker
```

Write an Ansible playbook to deploy the agent.

Below is an example of a basic installation.

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

#### Required settings

| variable name | Description |
|--------------------------------------------|---------------------------------------------------|
|`datasaker_api_key`|Enter the API Key.|
|`datasaker_agents`| A list of Host Agents to be installed on each host. <br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>(Default) `dsk-node -agent`|
|`datasaker_docker_agents`| A list of Docker Container Agents you want to install on each host. Host Agent installation is automatically disabled when Docker Container Agents are installed. <br>`dsk-docker-node-agent` `dsk-docker-trace-agent` `dsk-docker-log-agent` `dsk-docker-postgres-agent`<br>(Default) `dsk-docker- node-agent`|

<!--
#### Datasaker common settings
| variable name | Description |
|--------------------------------------------|---------------------------------------------------|
|`datagate_url`| Datasaker Datagate URL settings sent by Datasaker Agent. <br>(Default) `gate.kr.datasaker.io`|
|`datagate_trace_url`| `dsk-trace-agent` Datagate URL settings. <br>(Default) `datagate_url`|
|`datagate_trace_port`| `dsk-trace-agent` Datagate Port settings. <br>(Default) `31300` |
|`datagate_trace_timeout`| `dsk-trace-agent` Data transmission expiration time setting. <br>(Default) `5s` |
|`datagate_manifest_url`| `dsk-manifest-agent` Datagate URL settings. <br>(Default) `datagate_url`|
|`datagate_manifest_port`| `dsk-manifest-agent` Datagate Port settings. <br>(Default) `31301` |
|`datagate_manifest_timeout`| `dsk-manifest-agent` Data transmission expiration time setting. <br>(Default) `5s` |
|`datagate_metric_url`| `dsk-metric-agent` Datagate URL settings. <br>(Default) `datagate_url`|
|`datagate_metric_port`| `dsk-metric-agent` Datagate Port configuration. <br>(Default) `31302` |
|`datagate_metric_timeout`| `dsk-metric-agent` Data transfer expiration time setting. <br>(Default) `5s` |
|`datagate_plan_url`| `dsk-plan-agent` Datagate URL settings. <br>(Default) `datagate_url`|
|`datagate_plan_port`| `dsk-plan-agent` Datagate Port settings. <br>(Default) `31303` |
|`datagate_plan_timeout`| `dsk-plan-agent` Data transmission expiration time setting. <br>(Default) `5s` |
|`datagate_loggate_url`| `dsk-log-agent` Datagate URL settings. <br>(Default) `datagate_url`|
|`datagate_loggate_port`| `dsk-log-agent` Datagate Port settings. <br>(Default) `31304` |
|`datagate_loggate_timeout`| `dsk-log-agent` Data transfer expiration time setting. <br>(Default) `5s` |
|`datasaker_api_url`| Datasaker API Server URL. <br>(Default) `api.kr.datasaker.io`|
|`datasaker_api_send_interval`| Datasaker API Server Data transfer expiration time setting. <br>(Default) `1m` |
-->

#### Set up Docker Container Agent
| variable name | Description |
|--------------------------------------------|---------------------------------------------------|
|`datasaker_docker_config_path`| Datasaker Global Config location settings. <br> (Default) `~/.datasaker`|
|`datasaker_docker_global_config`| Datasaker Global Config name settings. <br> (Default) `~/.datasaker/config.yml`|
|`docker_default_path`| Set the Docker Log collection location to mount to the Datasaker Docker Log Agent. <br> (Default) `/var/lib/docker/containers/`|
|`datasaker_docker_path`| Datasaker Docker Agent Container location settings. <br> (Default) `/var/datasaker`|
|`container_agent_restart_policy`| `dsk-container-agent` Container Restart Policy settings. <br> (Default) `always`|
|`node_agent_restart_policy`| `dsk-node-agent` Container Restart Policy settings. <br> (Default) `always`|
|`trace_agent_restart_policy`| `dsk-trace-agent` Container Restart Policy settings. <br> (Default) `always`|
|`log_agent_restart_policy`| `dsk-log-agent` Container Restart Policy settings. <br> (Default) `always`|
|`postgres_agent_restart_policy`| `dsk-postgres-agent` Container Restart Policy settings. <br> (Default) `always`|
|`plan_postgres_agent_restart_policy`|`dsk-plan-postgres-agent` Container Restart Policy settings. <br> (Default) `always`|
|`container_agent_log_level`| `dsk-container-agent` Log Level settings. <br> (Default) `INFO` |
|`node_agent_log_level`| `dsk-node-agent` Log Level settings. <br> (Default) `INFO` |
|`trace_agent_log_level`| `dsk-trace-agent` Log Level settings. <br> (Default) `INFO` |
|`log_agent_log_level`| `dsk-log-agent` Log Level settings. <br> (Default) `INFO` |
|`postgres_agent_log_level`| `dsk-postgres-agent` Log Level settings. <br> (Default) `INFO` |
|`plan_postgres_agent_log_level`| `dsk-plan-postgres-agent` Log Level settings. <br> (Default) `INFO` |

<!--|`datasaker_docker_user`| Datasaker Docker Container Directory Ownership settings. <br> (Default) `datasaker`|
|`datasaker_docker_group`| Datasaker Docker Container Directory Group settings. <br> (Default) `datasaker`|
|`datasaker_docker_user_uid`| Set Datasaker Docker Container Agent User UID <br> (Default) `202306`|
|`datasaker_docker_user_gid`| Set Datasaker Docker Container Agent User GID <br> (Default) `202306`|
|`container_agent_image_tag`| `dsk-container-agent` Image tag configuration. <br> (Default) `latest`|
|`node_agent_image_tag`| `dsk-node-agent` Image tag configuration. <br> (Default) `latest`|
|`trace_agent_image_tag`| `dsk-trace-agent` Image tag configuration. <br> (Default) `latest`|
|`log_agent_image_tag`| `dsk-log-agent` Image tag configuration. <br> (Default) `latest`|
|`postgres_agent_image_tag`| `dsk-postgres-agent` Image tag configuration. <br> (Default) `latest`|
|`plan_postgres_agent_image_tag`| `dsk-plan-postgres-agent` Image tag configuration. <br> (Default) `latest`|-->

#### Datasaker Agent detailed settings
- Host Agent and Docker Container Agent use the same settings.

| variable name | Description |
|--------------------------------------------|---------------------------------------------------|
|`trace_sampling_rate`| Set the sampling rate applied to the collector in `dsk-trace-agent`.<br>- Collect all data when it is over 100.<br> (Default) `1`|
|`log_collects`| Log collection collection configuration settings in `dsk-log-agent`. Each item in the list contains the items below. |
|`log_collects[*].paths`| Set path for log collection in `dsk-log-agent`. <br> (Default) [`host-agent`]=`/var/log/*.log`, [`docker-agent`]=`/var/log/sample/*/*.log` |
|`log_collects[*].exclude_paths`| Set paths to exclude from log collection in `dsk-log-agent`. If no value is specified, all logs set in the collection path are collected. <br> (Default) `None` |
|`log_collects[*].keywords`| Set keywords to filter logs in `dsk-log-agent`. Collect all logs if no value is specified. <br> (Default) `None` |
|`log_collects[*].tag`| Custom tag in `dsk-log-agent`. <br> (Default) `Default` |
|`log_collects[*].service.name`| Set the service name collected by `dsk-log-agent`. <br> (Default) `default` |
|`log_collects[*].service.category`| Set service classification values ​​collected by `dsk-log-agent`. <br> `app` `database` `syslog` `etc` (Default) `etc` |
|`log_collects[*].service.type`| Service type settings collected by `dsk-log-agent`. <br> `postgres` `mysql` `java` `etc` (Default) `etc` |
|`log_collects[*].service.address`| Created when the service type collected by `dsk-log-agent` is Database <br>- Database host and port information |
|`postgres_user_name`| Set Postgres user ID in `dsk-postgres-agent`. <br> (Default) `None` |
|`postgres_user_password`| Set Postgres user password in `dsk-postgres-agent`. <br> (Default) `None` |
|`postgres_database_address`| Set Postgres address in `dsk-postgres-agent`. <br> (Default) `None` |
|`postgres_database_port`| Set Postgres port in `dsk-postgres-agent`. <br> (Default) `None` |
|`plan_postgres_user_name`| Set the Plan Postgres user ID in `dsk-plan-postgres-agent`. <br> (Default) `None` |
|`plan_postgres_user_password`| Set Plan Postgres user password in `dsk-plan-postgres-agent`. <br> (Default) `None` |
|`plan_postgres_database_address`| Set the Plan Postgres address in `dsk-plan-postgres-agent`. <br> (Default) `None` |
|`plan_postgres_database_port`| Set the Plan Postgres port in `dsk-plan-postgres-agent`. <br> (Default) `None` |
|`plan_postgres_database_name`| Configure the Plan Postgres database in `dsk-plan-postgres-agent`. <br> (Default) `None` |
|`plan_postgres_scrape_interval`| Set the Plan Postgres scrape interval in `dsk-plan-postgres-agent`. <br> (Default) `30s` |
|`plan_postgres_scrape_timeout`| Set Plan Postgres scrape timeout in `dsk-plan-postgres-agent`. <br> (Default) `5s` |
|`plan_postgres_slow_query_standard`| Set Plan Postgres slow query standard in `dsk-plan-postgres-agent`. <br> (Default) `5s` |
|`plan_postgres_executor_number`| Set the Plan Postgres executor number in `dsk-plan-postgres-agent`. <br> (Default) `10` |
|`plan_postgres_sender_number`| Set the Plan Postgres sender number in `dsk-plan-postgres-agent`. <br> (Default) `10` |
|`plan_postgres_activity_query_buffer`| `dsk-plan-postSet Plan Postgres activity query buffer in gres-agent`. <br> (Default) `50` |
|`plan_postgres_plan_sender_buffer`| Configure the Plan Postgres plan sender buffer in `dsk-plan-postgres-agent`. <br> (Default) `50` |

##### Ansible Playbook Detailed Settings Example
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
      -paths:
          - "/var/log/sample/*/*.log"
        exclude_paths: []
        keywords: []
        tag: "Default"
        service:
          name: "default"
          category: "etc"
          type: "etc"
      -paths:
          - "/var/log/sample/b4d5ac015a5a*/*.log"
        service:
          name: "docker-test"
          category: "database"
          type: "postgres"
          address: "0.0.0.0:5432"
```

## Uninstallation

You can uninstall Datasaker Agent.
datasaker_clean requires uninstall set to `True`.

| variable name | Description |
|--------------------------------------------|---------------------------------------------------|
|`uninstall`| Remove only agents created in `datasaker_agents` or `datasaker_docker_agents`. <br> (Default) `False` |
|`datasaker_clean`| Remove Agents written in `datasaker_agents` or `datasaker_docker_agents`, as well as created folders and configuration files. <br> (Default) `False` |

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