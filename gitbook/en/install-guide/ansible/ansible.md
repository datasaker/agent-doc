# Ansible Datasaker Role

You can install Datasaker Agent using Ansible.

## Requirements

- Requires Ansible v2.6+.
- Supports most Debian Linux distributions.
- Supports most Redhat Linux distributions.
- Supports Amazon Linux 2 distribution.


## Installation

Install the Datasaker role in Ansible Galaxy.
```shell
ansible-galaxy install dsk_bot.datasaker
```
Write an Ansible playbook to deploy the agent.


****When installing `dsk-log-agent`, `fluent-bit` is automatically installed.***


Below is an example of a basic installation.

#### Host Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent","dsk-log-agent"] 
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
### Required settings

| variable name | Description | Default |
|--------------------------------------------|---------------------------------------------------|---------------------------------------------------|
|`datasaker_api_key`|Enter the API Key.|
|`datasaker_agents`| A list of Host Agents to be installed on each host. <br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>| `dsk-node-agent`|
|`datasaker_docker_agents`| A list of Docker Container Agents you want to install on each host. <br>Adding Docker Container Agents automatically disables Host Agent installation. <br>`dsk-docker-node-agent` `dsk-docker-trace-agent` `dsk-docker-log-agent` `dsk-docker-postgres-agent`<br>| `dsk-docker-node-agent`|
<!--
#### Datasaker common settings
| variable name | Description | Default |
|--------------------------------------------|---------------------------------------------------|---------------------------------------------------|
|`datagate_url`| Datasaker Datagate URL settings sent by Datasaker Agent. <br>| `gate.kr.datasaker.io` |
|`datagate_trace_url`| `dsk-trace-agent` Datagate URL settings. <br>| `datagate_url`|
|`datagate_trace_port`| `dsk-trace-agent` Datagate Port settings. <br>| `31300` |
|`datagate_trace_timeout`| `dsk-trace-agent` Data transmission expiration time setting. <br>| `5s` |
|`datagate_manifest_url`| `dsk-manifest-agent` Datagate URL settings. <br>| `datagate_url`|
|`datagate_manifest_port`| `dsk-manifest-agent` Datagate Port settings. <br>| `31301`|
|`datagate_manifest_timeout`| `dsk-manifest-agent` Data transmission expiration time setting. <br>| `5s` |
|`datagate_metric_url`| `dsk-metric-agent` Datagate URL settings. <br>| `datagate_url`|
|`datagate_metric_port`| `dsk-metric-agent` Datagate Port configuration. <br>| `31302`|
|`datagate_metric_timeout`| `dsk-metric-agent` Data transfer expiration time setting. <br>| `5s` |
|`datagate_plan_url`| `dsk-plan-agent` Datagate URL settings. <br>| `datagate_url`|
|`datagate_plan_port`| `dsk-plan-agent` Datagate Port settings. <br>| `31303`|
|`datagate_plan_timeout`| `dsk-plan-agent` Data transmission expiration time setting. <br>| `5s` |
|`datagate_loggate_url`| `dsk-log-agent` Datagate URL settings. <br>| `datagate_url`|
|`datagate_loggate_port`| `dsk-log-agent` Datagate Port settings. <br>| `31304`|
|`datagate_loggate_timeout`| `dsk-log-agent` Data transfer expiration time setting. <br>| `5s` |
|`datasaker_api_url`| Datasaker API Server URL. <br>| `api.kr.datasaker.io`|
|`datasaker_api_send_interval`| Datasaker API Server Data transfer expiration time setting. <br>| `1m` |
-->

### Set up Docker Container Agent
| variable name | Description | Default |
|--------------------------------------------|---------------------------------------------------|---------------------------------------------------|
|`datasaker_docker_config_path`| Datasaker Global Config location settings. <br> | `~/.datasaker`|
|`datasaker_docker_global_config`| Datasaker Global Config name settings. <br> | `~/.datasaker/config.yml`|
|`docker_default_path`| Set the Docker Log collection location to mount to the Datasaker Docker Log Agent. <br> | `/var/lib/docker/containers/`|
|`datasaker_docker_path`| Datasaker Docker Agent Container location settings. <br> | `/var/datasaker`|
|`container_agent_restart_policy`| `dsk-container-agent` Container Restart Policy settings. <br> | `always` |
|`node_agent_restart_policy`| `dsk-node-agent` Container Restart Policy settings. <br> | `always` |
|`trace_agent_restart_policy`| `dsk-trace-agent` Container Restart Policy settings. <br> | `always` |
|`log_agent_restart_policy`| `dsk-log-agent` Container Restart Policy settings. <br> | `always` |
|`postgres_agent_restart_policy`| `dsk-postgres-agent` Container Restart Policy settings. <br> | `always` |
|`plan_postgres_agent_restart_policy`| `dsk-plan-postgres-agent` Container Restart Policy settings. <br> | `always` |
|`container_agent_log_level`| `dsk-container-agent` Log Level settings. <br> | `INFO`|
|`node_agent_log_level`| `dsk-node-agent` Log Level settings. <br> | `INFO`|
|`trace_agent_log_level`| `dsk-trace-agent` Log Level settings. <br> | `INFO`|
|`log_agent_log_level`| `dsk-log-agent` Log Level settings. <br> | `INFO`|
|`postgres_agent_log_level`| `dsk-postgres-agent` Log Level settings. <br> | `INFO`|
|`plan_postgres_agent_log_level`| `dsk-plan-postgres-agent` Log Level settings. <br> | `INFO`|

<!--|`datasaker_docker_user`| Datasaker Docker Container Directory Ownership settings. <br> | `datasaker`|
|`datasaker_docker_group`| Datasaker Docker Container Directory Group settings. <br> | `datasaker`|
|`datasaker_docker_user_uid`| Set Datasaker Docker Container Agent User UID <br> | `202306`|
|`datasaker_docker_user_gid`| Set Datasaker Docker Container Agent User GID <br> | `202306`|
|`container_agent_image_tag`| `dsk-container-agent` Image tag configuration. <br> | `latest`|
|`node_agent_image_tag`| `dsk-node-agent` Image tag configuration. <br> | `latest`||`trace_agent_image_tag`| `dsk-trace-agent` Image tag configuration. <br> | `latest`|
|`log_agent_image_tag`| `dsk-log-agent` Image tag configuration. <br> | `latest`|
|`postgres_agent_image_tag`| `dsk-postgres-agent` Image tag configuration. <br> | `latest`|
|`plan_postgres_agent_image_tag`| `dsk-plan-postgres-agent` Image tag configuration. <br> | `latest`|-->

### Datasaker Agent detailed settings
- Host Agent and Docker Container Agent use the same settings.

| variable name | Description | Default |
|--------------------------------------------|---------------------------------------------------|---------------------------------------------------|
|`trace_sampling_rate`| Set the sampling rate applied to the collector in `dsk-trace-agent`.<br>- Collect all data when it is over 100.<br> | `10` |
|`logs[*].service`|Set the service name of the log collection destination.|`default`|
|`logs[*].tag`|Tag setting for log collection target.|`None`|
|`logs[*].keyword`|Set log collection keywords. Only collect logs containing keywords.|`None`|
|`logs[*].multiline.format`|Set multiline log format (eg go, java, ruby, python).|`None`|
|`logs[*].multiline.pattern`|Set multiline log pattern. (e.g. ^\d{4}-\d{2}-\d{2}).|`None`|
|`logs[*].masking[*].pattern`|Set the log pattern to mask. (Example: ^\d{4}-\d{2}-\d{2}) User-customized regular expression pattern can be used.|`None`|
|`logs[*].masking[*].replace`|Set the string to be replaced with the masking pattern. (e.g. ******).|`None`|
|`logs[*].collect.type`|Set log collection method (write one of `file` or `driver` value).|`file`|
|`logs[*].collect.category`|Configure service category (write one value among `app`, `database`, `syslog`, `etc`).|`etc`|
|`logs[*].collect.address`|Database host and port information setting (set when service classification is database).|`None`|
|`logs[*].collect.file.paths`|Set the log collection target path. Example: /var/log/sample/*.log.|`['/var/log/*.log']`|
|`logs[*].collect.file.exclude_paths`|Set log collection exclusion target path.|`None`|
|`custom_log_volume`| Mount path with logs to collect when using Docker.|`/var/lib/docker/containers/`|
|`postgres_user_name`| Set Postgres user ID in `dsk-postgres-agent`. <br> | `None` |
|`postgres_user_password`| Set Postgres user password in `dsk-postgres-agent`. <br> | `None` |
|`postgres_database_address`| Set Postgres address in `dsk-postgres-agent`. <br> | `None` |
|`postgres_database_port`| Set Postgres port in `dsk-postgres-agent`. <br> | `None` |
|`plan_postgres_user_name`| Set the Plan Postgres user ID in `dsk-plan-postgres-agent`. <br> | `None` |
|`plan_postgres_user_password`| Set Plan Postgres user password in `dsk-plan-postgres-agent`. <br> | `None` |
|`plan_postgres_database_address`| Set the Plan Postgres address in `dsk-plan-postgres-agent`. <br> | `None` |
|`plan_postgres_database_port`| Set the Plan Postgres port in `dsk-plan-postgres-agent`. <br> | `None` |
|`plan_postgres_database_name`| Configure the Plan Postgres database in `dsk-plan-postgres-agent`. <br> | `None` |
|`plan_postgres_scrape_interval`| Set the Plan Postgres scrape interval in `dsk-plan-postgres-agent`. <br> | `30s` |
|`plan_postgres_scrape_timeout`| Set Plan Postgres scrape timeout in `dsk-plan-postgres-agent`. <br> | `5s` |
|`plan_postgres_slow_query_standard`| Set Plan Postgres slow query standard in `dsk-plan-postgres-agent`. <br> | `5s` |
|`plan_postgres_executor_number`| Set the Plan Postgres executor number in `dsk-plan-postgres-agent`. <br> | `10` |
|`plan_postgres_sender_number`| Set the Plan Postgres sender number in `dsk-plan-postgres-agent`. <br> | `10` |
|`plan_postgres_activity_query_buffer`| Set the Plan Postgres activity query buffer in `dsk-plan-postgres-agent`. <br> | `50` |
|`plan_postgres_plan_sender_buffer`| Configure the Plan Postgres plan sender buffer in `dsk-plan-postgres-agent`. <br> | `50` |

#### Ansible Playbook Detailed Configuration Example (Linux)
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents:
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
```
#### Ansible Playbook Detailed Configuration Example (Docker)
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
          - /var/lib/docker/containers/*/*.log
    custom_log_volume:
    - /var/log/
    - /var/lib/docker/containers
```
## Uninstallation

You can uninstall Datasaker Agent.
datasaker_clean requires uninstall set to `True`.

| variable name | Description | Default |
|--------------------------------------------|---------------------------------------------------|---------------------------------------------------|
|`uninstall`| Remove only agents created in `datasaker_agents` or `datasaker_docker_agents`. <br> | `False`|
|`datasaker_clean`| Remove Agents written in `datasaker_agents` or `datasaker_docker_agents`, as well as created folders and configuration files. <br> | `False`|

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