# Ansible Datasaker Role

The Ansible Datasaker role installs and configures the Datasaker Agent and integrations.

## Setup

### Requirements

- Requires Ansible v2.6+.
- Supports most Debian Linux distributions.
- Supports most RedHat Linux distributions.
- Supports Amazon Linux 2 distributions.

### Installation

Install the [Datasaker role] from Ansible Galaxy on your Ansible server:

```shell
ansible-galaxy install dsk_bot.datasaker
```

To deploy the Datasaker Agent on hosts, add the Datasaker role and your API key to your playbook:

*** When installing `dsk-log-agent`, `fluent-bit` is automatically installed. ***

In this example:

###### Host Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent","dsk-log-agent"] 
```
###### Docker Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents: ["dsk-docker-node-agent","dsk-docker-log-agent"]
```

#### Base Role variables

| Variable                                   | Description                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`datasaker_api_key`|Your Datasaker API key.|
|`datasaker_agents`|Set to Datasaker Host Agent.<br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>| `dsk-node-agent`|
|`datasaker_docker_agents`|Set to Datasaker Docker Agent.<br>`dsk-docker-node-agent` `dsk-docker-trace-agent` `dsk-docker-log-agent` `dsk-docker-postgres-agent`<br>| `dsk-docker-node-agent`|

<!--
#### Agent Global Config Role variables
| Variable                                   | Description                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`datagate_url`|The site of the Datasaker intake to send Agent data to. <br>| `gate.kr.datasaker.io`|
|`datagate_trace_url`|Override the `dsk-trace-agent` datagate url. <br>| `datagate_url`|
|`datagate_trace_port`|Override the `dsk-trace-agent` datagate port. <br>| `31300`|
|`datagate_trace_timeout`|Override the `dsk-trace-agent` data expiration time. <br>| `5s`|
|`datagate_manifest_url`|Override the `dsk-manifest-agent` datagate url. <br>| `datagate_url`|
|`datagate_manifest_port`|Override the `dsk-manifest-agent` datagate port. <br>| `31301`|
|`datagate_manifest_timeout`|Override the `dsk-manifest-agent` data expiration time. <br>| `5s`|
|`datagate_metric_url`|Override the `dsk-metric-agent` datagate url. <br>| `datagate_url`|
|`datagate_metric_port`|Override the `dsk-metric-agent` datagate port. <br>| `31302`|
|`datagate_metric_timeout`|Override the `dsk-metric-agent` data expiration time. <br>| `5s`|
|`datagate_plan_url`|Override the `dsk-plan-agent` datagate url. <br>| `datagate_url`|
|`datagate_plan_port`|Override the `dsk-plan-agent` datagate port. <br>| `31303`|
|`datagate_plan_timeout`|Override the `dsk-plan-agent` data expiration time. <br>| `5s`|
|`datagate_loggate_url`|Override the `dsk-log-agent` datagate url. <br>| `datagate_url`|
|`datagate_loggate_port`|Override the `dsk-log-agent` datagate port. <br>| `31304`|
|`datagate_loggate_timeout`|Override the `dsk-log-agent` data expiration time. <br>| `5s`|
|`datasaker_api_url`|Override the datasaker api server url. <br>| `api.kr.datasaker.io`|
|`datasaker_api_send_interval`|Override the datasaker api server data expiration time. <br>| `1m`|
-->

#### Docker Agent Role variables
| Variable                                   | Description                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`datasaker_docker_config_path`| Override the datasaker global config path. <br> | `~/.datasaker`|
|`datasaker_docker_global_config`| Override the datasaker global config file name. <br> | `~/.datasaker/config.yml`|
|`docker_default_path`| Override the docker containers path. <br> | `/var/lib/docker/containers/`|
|`datasaker_docker_path`| Override the datasaker docker agent containers path. <br> | `/var/datasaker`|
|`container_agent_restart_policy`| Override the restart policy for a `dsk-container-agent` container <br> | `always`|
|`node_agent_restart_policy`| Override the  restart policy for a `dsk-node-agent` container <br> | `always`|
|`trace_agent_restart_policy`| Override the  restart policy for a `dsk-trace-agent` container <br> | `always`|
|`log_agent_restart_policy`| Override the  restart policy for a `dsk-log-agent` container <br> | `always`|
|`postgres_agent_restart_policy`| Override the  restart policy for a `dsk-postgres-agent` container <br> | `always`|
|`plan_postgres_agent_restart_policy`| Override the  restart policy for a `dsk-plan-postgres-agent` container <br> | `always`|
|`container_agent_log_level`| Override the `dsk-container-agent` log level <br> | `INFO`|
|`node_agent_log_level`| Override the `dsk-node-agent` log level <br> | `INFO`|
|`trace_agent_log_level`| Override the `dsk-trace-agent` log level <br> | `INFO`|
|`log_agent_log_level`| Override the `dsk-log-agent` log level <br> | `INFO`|
|`postgres_agent_log_level`| Override the `dsk-postgres-agent` log level <br> | `INFO`|
|`plan_postgres_agent_log_level`| Override the `dsk-plan-postgres-agent` log level <br> | `INFO`|


<!--
|`datasaker_docker_user`| Override Owner in the datasaker docker agent containers directory. <br> | `datasaker`|
|`datasaker_docker_group`| Override Group in the datasaker docker agent containers directory. <br> | `datasaker`|
|`datasaker_docker_user_uid`| Override uid in the datasaker user. <br> | `202306`|
|`datasaker_docker_user_gid`| Override gid in the datasaker user. <br> | `202306`|
|`container_agent_image_tag`| Override the `dsk-container-agent` Image tag. <br> | `latest`|
|`node_agent_image_tag`| Override the `dsk-node-agent` Image tag. <br> | `latest`|
|`trace_agent_image_tag`| Override the `dsk-trace-agent` Image tag. <br> | `latest`|
|`log_agent_image_tag`| Override the `dsk-log-agent` Image tag. <br> | `latest`|
|`postgres_agent_image_tag`| Override the `dsk-postgres-agent` Image tag. <br> | `latest`|
|`plan_postgres_agent_image_tag`| Override the `dsk-plan-postgres-agent` Image tag. <br> | `latest`|
-->

#### Agents Setting Role variables
| Variable                                   | Description                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`trace_sampling_rate`| Override The `dsk-trace-agent` sampling rate applied to the collector.<br>- When set to 100 or higher, all data is collected.<br> | `10`|
|`logs[*].service`|Defines the service name of the log collection target.|`default`|
|`logs[*].tag`|Sets the tag of the log collection target.|`None`|
|`logs[*].keyword`|Sets the keyword for log collection. Only logs that include the keyword are collected.|`None`|
|`logs[*].multiline.format`|Sets the multiline log format (e.g.: go, java, ruby, python).|`None`|
|`logs[*].multiline.pattern`|Sets the multiline log pattern. (e.g.: ^\d{4}-\d{2}-\d{2}).|`None`|
|`logs[*].masking[*].pattern`|Sets the log pattern to be masked. (e.g.: ^\d{4}-\d{2}-\d{2}) User-defined regular expression patterns are possible.|`None`|
|`logs[*].masking[*].replace`|Sets the string that the masking pattern will be replaced with. (e.g.: *****).|`None`|
|`logs[*].collect.type`|Sets the method of log collection (Choose one from file, driver).|`file`|
|`logs[*].collect.category`|Sets the service category (Choose one from app, database, syslog, etc).|`etc`|
|`logs[*].collect.address`|Sets the database host and port information (required if service category is database).|`None`|
|`logs[*].collect.file.paths`|Sets the paths for log collection. Example: /var/log/sample/.log.|`['/var/log/*.log']`|
|`logs[*].collect.file.exclude_paths`|Sets the paths to be excluded from log collection.|`None`|
|`postgres_user_name`| Enter the Postgres user ID. <br> | `None` |
|`postgres_user_password`| Enter the Postgres user password. <br> | `None` |
|`postgres_database_address`| Enter the Postgres address. <br> | `None` |
|`postgres_database_port`| Enter the Postgres port. <br> | `None` |
|`plan_postgres_user_name`| Enter the Plan Postgres user ID. <br> | `None` |
|`plan_postgres_user_password`| Enter the Plan Postgres user password. <br> | `None` |
|`plan_postgres_database_address`| Enter the Plan Postgres address. <br> | `None` |
|`plan_postgres_database_port`| Enter the Plan Postgres port. <br> | `None` |
|`plan_postgres_database_name`| Enter the Plan Postgres database. <br> | `None` |
|`plan_postgres_scrape_interval`| Override the Plan Postgres scrape interval <br> | `30s` |
|`plan_postgres_scrape_timeout`| Override the Plan Postgres scrape timeout <br> | `5s` |
|`plan_postgres_slow_query_standard`| Override the Plan Postgres slow query standard <br> | `5s` |
|`plan_postgres_executor_number`| Override the Plan Postgres executor number <br> | `10` |
|`plan_postgres_sender_number`| Override the Plan Postgres sender number <br> | `10` |
|`plan_postgres_activity_query_buffer`| Override the Plan Postgres activity query buffer <br> | `50` |
|`plan_postgres_plan_sender_buffer`| Override the Plan Postgres plan sender buffer <br> | `50` |


In this example:

###### Ansible Playbook Setting Example (Linux)
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents:
      - "dsk-node-agent"
      - "dsk-trace-agent"
      - "dsk-log-agent"
      - "dsk-postgres-agent"
    postgres_user_name: sample
    postgres_user_password: 1q2w3e4r
    postgres_database_address: 0.0.0.0
    postgres_database_port: 5432
    plan_postgres_user_name: sample
    plan_postgres_user_password: 1q2w3e4r
    plan_postgres_database_address: 0.0.0.0
    plan_postgres_database_name: sample
    plan_postgres_database_port: 5432
    logs:
      - collect:
          type: file
          file:
            paths: 
              - /var/log/*.log
              - /datasaker/log/*.log
```

###### Ansible Playbook Setting Example (Docker)
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
    logs:
      - collect:
          type: file
          file:
            paths: 
              - /var/log/*.log
              - /datasaker/log/*.log
```

## Uninstallation

Datasaker Agent can be uninstalled.
For this, datasaker_clean should be set to True.

| Variable                                   | Description                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`uninstall`| Only removes the agents specified in `datasaker_agents` or `datasaker_docker_agents`. <br>| `False`|
|`datasaker_clean`|	Removes the agents specified in `datasaker_agents` or `datasaker_docker_agents` along with any generated folders and configuration files. <br> | `False`|

In this example:

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
