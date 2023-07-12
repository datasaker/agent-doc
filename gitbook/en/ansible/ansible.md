# Ansible Datasaker Role

You can install Datasaker Agent using Ansible.

## Requirements

* Requires Ansible v2.6+.
* Supports most Debian Linux distributions.
* Supports Amazon Linux 2 distribution.

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
| -------------------------------------------- | -------------------------------------------------- -------------------------------------------------- -------------------------------------------------- -------------------------------------------------- -------------------------------------------------- -------------------------------------------------- ------- |
| `datasaker_api_key` | Enter your API Key. |
| `datasaker_agents` | <p>A list of host agents to be installed on each host.<br><code>dsk-node-agent</code> <code>dsk-trace-agent</code> <code>dsk-log-agent< /code> <code>dsk-postgres-agent</code> <code>dsk-plan-postgres-agent</code><br>(Default) <code>dsk-node-agent</code></p > |
| `datasaker_docker_agents` | <p>A list of Docker Container Agents you want to install on each host. Host Agent installation is automatically disabled when Docker Container Agents are added.<br><code>dsk-docker-node-agent</code> <code>dsk-docker-trace-agent</code> <code>dsk- docker-log-agent</code> <code>dsk-docker-postgres-agent</code><br>(Default) <code>dsk-docker-node-agent</code></p> |
| \<!-- | |
| #### Datasaker common settings | |
| variable name | Description |
| -------------------------------------------- | -------------------------------------------------- |
| `datagate_url` | <p>Set the Datasaker Datagate URL transmitted by the Datasaker Agent.<br>(Default) <code>gate.kr.datasaker.io</code></p> |
| `datagate_trace_url` | <p><code>dsk-trace-agent</code> Datagate URL setting.<br>(Default) <code>datagate_url</code></p> |
| `datagate_trace_port` | <p><code>dsk-trace-agent</code> Datagate Port settings.<br>(Default) <code>31300</code></p> |
| `datagate_trace_timeout` | <p><code>dsk-trace-agent</code> Set data transfer expiration time.<br>(Default) <code>5s</code></p> |
| `datagate_manifest_url` | <p><code>dsk-manifest-agent</code> Set Datagate URL.<br>(Default) <code>datagate_url</code></p> |
| `datagate_manifest_port` | <p><code>dsk-manifest-agent</code> Set Datagate Port.<br>(Default) <code>31301</code></p> |
| `datagate_manifest_timeout` | <p><code>dsk-manifest-agent</code> Set data transfer expiration time.<br>(Default) <code>5s</code></p> |
| `datagate_metric_url` | <p><code>dsk-metric-agent</code> Set Datagate URL.<br>(Default) <code>datagate_url</code></p> |
| `datagate_metric_port` | <p><code>dsk-metric-agent</code> Set Datagate Port.<br>(Default) <code>31302</code></p> |
| `datagate_metric_timeout` | <p><code>dsk-metric-agent</code> Set data transfer expiration time.<br>(Default) <code>5s</code></p> |
| `datagate_plan_url` | <p><code>dsk-plan-agent</code> Set Datagate URL.<br>(Default) <code>datagate_url</code></p> |
| `datagate_plan_port` | <p><code>dsk-plan-agent</code> Set Datagate Port.<br>(Default) <code>31303</code></p> |
| `datagate_plan_timeout` | <p><code>dsk-plan-agent</code> Set data transfer expiration time.<br>(Default) <code>5s</code></p> |
| `datagate_loggate_url` | <p><code>dsk-log-agent</code> Set Datagate URL.<br>(Default) <code>datagate_url</code></p>|
| `datagate_loggate_port` | <p><code>dsk-log-agent</code> Datagate Port settings.<br>(Default) <code>31304</code></p> |
| `datagate_loggate_timeout` | <p><code>dsk-log-agent</code> Set data transfer expiration time.<br>(Default) <code>5s</code></p> |
| `datasaker_api_url` | <p>Datasaker API Server URL.<br>(Default) <code>api.kr.datasaker.io</code></p> |
| `datasaker_api_send_interval` | <p>Datasaker API Server Data transfer expiration time setting.<br>(Default) <code>1m</code></p> |
| --> | |

#### Set up Docker Container Agent

| variable name | Description |
| ------------------------------------ | -------------------------------------------------- -------------------------------------------------- -------------- |
| `datasaker_docker_config_path` | <p>Set Datasaker Global Config location.<br>(Default) <code>~/.datasaker</code></p> |
| `datasaker_docker_global_config` | <p>Set Datasaker Global Config name.<br>(Default) <code>~/.datasaker/config.yml</code></p> |
| `docker_default_path` | <p>Set the Docker Log collection location to mount to the Datasaker Docker Log Agent.<br>(Default) <code>/var/lib/docker/containers/</code></p> |
| `datasaker_docker_path` | <p>Set Datasaker Docker Agent Container location.<br>(Default) <code>/var/datasaker</code></p> |
| `container_agent_restart_policy` | <p><code>dsk-container-agent</code> Set Container Restart Policy.<br>(Default) <code>always</code></p> |
| `node_agent_restart_policy` | <p><code>dsk-node-agent</code> Set Container Restart Policy.<br>(Default) <code>always</code></p> |
| `trace_agent_restart_policy` | <p><code>dsk-trace-agent</code> Set Container Restart Policy.<br>(Default) <code>always</code></p> |
| `log_agent_restart_policy` | <p><code>dsk-log-agent</code> Set Container Restart Policy.<br>(Default) <code>always</code></p> |
| `postgres_agent_restart_policy` | <p><code>dsk-postgres-agent</code> Set Container Restart Policy.<br>(Default) <code>always</code></p> |
| `plan_postgres_agent_restart_policy` | <p><code>dsk-plan-postgres-agent</code> Set Container Restart Policy.<br>(Default) <code>always</code></p> |
| `container_agent_log_level` | <p><code>dsk-container-agent</code> Set Log Level.<br>(Default) <code>INFO</code></p> |
| `node_agent_log_level` | <p><code>dsk-node-agent</code> Set Log Level.<br>(Default) <code>INFO</code></p> |
| `trace_agent_log_level` | <p><code>dsk-trace-agent</code> Set Log Level.<br>(Default) <code>INFO</code></p> |
| `log_agent_log_level` | <p><code>dsk-log-agent</code> Set Log Level.<br>(Default) <code>INFO</code></p> |
| `postgres_agent_log_level` | <p><code>dsk-postgres-agent</code> Set Log Level.<br>(Default) <code>INFO</code></p> |
| `plan_postgres_agent_log_level` | <p><code>dsk-plan-postgres-agent</code> Set Log Level.<br>(Default) <code>INFO</code></p> |

#### Datasaker Agent detailed settings

* Host Agent and Docker Container Agent use the same settings.

| variable name | Description |
| ------------------------------------- | -------------------------------------------------- -------------------------------------------------- -------------------------------------------------- ------------------------------------ |
| `trace_sampling_rate` | <p>Sets the sampling rate applied to the collector in <code>dsk-trace-agent</code>.<br>- All data is collected when it is over 100.<br>(Default) <code>1</code> </p> |
| `log_collects` | Log collection collection configuration settings in `dsk-log-agent`. Each item in the list contains the items below. |
| `log_collects[*].paths` | <p>Set path for log collection in <code>dsk-log-agent</code>.<br>(Default) [<code>host-agent</code>]=<code>/var/log/ *.log</code>, [<code>docker-agent</code>]=<code>/var/log/sample/*/*.log</code></p> |
| `log_collects[*].exclude_paths` | <p>Set paths to exclude from log collection in <code>dsk-log-agent</code>. If no value is specified, all logs set in the collection path are collected.<br>(Default) <code>None</code></p> |
| `log_collects[*].keywords` | <p>Set keywords to filter logs in <code>dsk-log-agent</code>. Collect all logs if no value is specified.<br>(Default) <code>None</code></p> |
| `log_collects[*].tag` | <p>Custom tag in <code>dsk-log-agent</code>.<br>(Default) <code>Default</code></p> |
| `log_collects[*].service.name` | <p>Set the service name collected by <code>dsk-log-agent</code>.<br>(Default) <code>default</code></p> |
| `log_collects[*].service.category` | <p>Configure service classification values ​​collected by <code>dsk-log-agent</code>.<br><code>app</code> <code>database</code> <code>syslog</code> <code>etc</code> (Default) <code>etc</code></p> |
| `log_collects[*].service.type` | <p>Configure the service type collected by <code>dsk-log-agent</code>.<br><code>postgres</code> <code>mysql</code> <code>java</code> < code>etc</code> (Default) <code>etc</code></p> |
| `log_collects[*].service.address` | <p>Written when the service type collected by <code>dsk-log-agent</code> is Database<br>- Database host and port information</p> |
| `postgres_user_name` | <p>Set Postgres user ID in <code>dsk-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `postgres_user_password` | <p>Set Postgres user password in <code>dsk-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `postgres_database_address` | <p>Set Postgres address in <code>dsk-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `postgres_database_port` | <p>Set Postgres port in <code>dsk-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `plan_postgres_user_name` | <p>Set the Plan Postgres user ID in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `plan_postgres_user_password` | <p>Set Plan Postgres user password in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `plan_postgres_database_address` | <p>Set Plan Postgres address in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `plan_postgres_database_port` | <p>Configure Plan Postgres port in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `plan_postgres_database_name` | <p>Configure Plan Postgres database in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>None</code></p> |
| `plan_postgres_scrape_interval` | <p>Set the Plan Postgres scrape interval in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>30s</code></p> |
| `plan_postgres_scrape_timeout` | <p>Set Plan Postgres scrape timeout in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>5s</code></p> |
| `plan_postgres_slow_query_standard` | <p>Set Plan Postgres slow query standard in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>5s</code></p> |
| `plan_postgres_executor_number` | <p>Set Plan Postgres executor number in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>10</code></p> |
| `plan_postgres_sender_number` | <p>Set Plan Postgres sender number in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>10</code></p> |
| `plan_postgres_activity_query_buffer` | <p>Set the Plan Postgres activity query buffer in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>50</code></p> |
| `plan_postgres_plan_sender_buffer` | <p>Configure Plan Postgres plan sender buffer in <code>dsk-plan-postgres-agent</code>.<br>(Default) <code>50</code></p> |

**Ansible Playbook Detailed Configuration Example**

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

You can uninstall Datasaker Agent. datasaker\_clean requires uninstall set to `True`.

| variable name | Description |
| ----------------- | -------------------------------------------------- -------------------------------------------------- -------------------------------------------- |
| `uninstall` | <p>Remove only agents written in <code>datasaker_agents</code> or <code>datasaker_docker_agents</code>.<br>(Default) <code>False</code></p> |
| `datasaker_clean` | <p>Remove Agents written in <code>datasaker_agents</code> or <code>datasaker_docker_agents</code>, as well as created folders and configuration files.<br>(Default) <code>False</code></p > |

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