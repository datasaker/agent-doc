# Ansible Datasaker Role

The Ansible Datasaker role installs and configures the Datasaker Agent and integrations.

## Setup

### Requirements

- Requires Ansible v2.6+.
- Supports most Debian Linux distributions.
- Supports Amazon Linux 2 distributions.

### Installation

Install the [Datasaker role] from Ansible Galaxy on your Ansible server:

```shell
ansible-galaxy install dsk_bot.datasaker
```

To deploy the Datasaker Agent on hosts, add the Datasaker role and your API key to your playbook:

###### Host Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent"] 
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

| Variable                                   | Description                                      |
|--------------------------------------------|--------------------------------------------------|
|`datasaker_api_key`|Your Datasaker API key.|
|`datasaker_agents`|Set to Datasaker Host Agent.<br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>(Default) `dsk-node-agent`|
|`datasaker_docker_agents`|Set to Datasaker Docker Agent.<br>`dsk-docker-node-agent` `dsk-docker-trace-agent` `dsk-docker-log-agent` `dsk-docker-postgres-agent`<br>(Default) `dsk-docker-node-agent`|
<!--
#### Agent Global Config Role variables
| Variable                                   | Description                                      |
|--------------------------------------------|--------------------------------------------------|
|`datagate_url`|The site of the Datasaker intake to send Agent data to. <br>(Default) `gate.kr.datasaker.io`|
|`datagate_trace_url`|Override the `dsk-trace-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_trace_port`|Override the `dsk-trace-agent` datagate port. <br>(Default) `31300`|
|`datagate_trace_timeout`|Override the `dsk-trace-agent` data expiration time. <br>(Default) `5s`|
|`datagate_manifest_url`|Override the `dsk-manifest-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_manifest_port`|Override the `dsk-manifest-agent` datagate port. <br>(Default) `31301`|
|`datagate_manifest_timeout`|Override the `dsk-manifest-agent` data expiration time. <br>(Default) `5s`|
|`datagate_metric_url`|Override the `dsk-metric-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_metric_port`|Override the `dsk-metric-agent` datagate port. <br>(Default) `31302`|
|`datagate_metric_timeout`|Override the `dsk-metric-agent` data expiration time. <br>(Default) `5s`|
|`datagate_plan_url`|Override the `dsk-plan-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_plan_port`|Override the `dsk-plan-agent` datagate port. <br>(Default) `31303`|
|`datagate_plan_timeout`|Override the `dsk-plan-agent` data expiration time. <br>(Default) `5s`|
|`datagate_loggate_url`|Override the `dsk-log-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_loggate_port`|Override the `dsk-log-agent` datagate port. <br>(Default) `31304`|
|`datagate_loggate_timeout`|Override the `dsk-log-agent` data expiration time. <br>(Default) `5s`|
|`datasaker_api_url`|Override the datasaker api server url. <br>(Default) `api.kr.datasaker.io`|
|`datasaker_api_send_interval`|Override the datasaker api server data expiration time. <br>(Default) `1m`|
-->

#### Docker Agent Role variables
| Variable                                   | Description                                      |
|--------------------------------------------|--------------------------------------------------|
|`datasaker_docker_config_path`| Override the datasaker global config path. <br> (Default) `~/.datasaker`|
|`datasaker_docker_global_config`| Override the datasaker global config file name. <br> (Default) `~/.datasaker/config.yml`|
|`docker_default_path`| Override the docker containers path. <br> (Default) `/var/lib/docker/containers/`|
|`datasaker_docker_path`| Override the datasaker docker agent containers path. <br> (Default) `/var/datasaker`|
|`container_agent_restart_policy`| Override the restart policy for a `dsk-container-agent` container <br> (Default) `always`|
|`node_agent_restart_policy`| Override the  restart policy for a `dsk-node-agent` container <br> (Default) `always`|
|`trace_agent_restart_policy`| Override the  restart policy for a `dsk-trace-agent` container <br> (Default) `always`|
|`log_agent_restart_policy`| Override the  restart policy for a `dsk-log-agent` container <br> (Default) `always`|
|`postgres_agent_restart_policy`| Override the  restart policy for a `dsk-postgres-agent` container <br> (Default) `always`|
|`plan_postgres_agent_restart_policy`| Override the  restart policy for a `dsk-plan-postgres-agent` container <br> (Default) `always`|
|`container_agent_log_level`| Override the `dsk-container-agent` log level <br> (Default) `INFO`|
|`node_agent_log_level`| Override the `dsk-node-agent` log level <br> (Default) `INFO`|
|`trace_agent_log_level`| Override the `dsk-trace-agent` log level <br> (Default) `INFO`|
|`log_agent_log_level`| Override the `dsk-log-agent` log level <br> (Default) `INFO`|
|`postgres_agent_log_level`| Override the `dsk-postgres-agent` log level <br> (Default) `INFO`|
|`plan_postgres_agent_log_level`| Override the `dsk-plan-postgres-agent` log level <br> (Default) `INFO`|

<!--|`datasaker_docker_user`| Override Owner in the datasaker docker agent containers directory. <br> (Default) `datasaker`|
|`datasaker_docker_group`| Override Group in the datasaker docker agent containers directory. <br> (Default) `datasaker`|
|`datasaker_docker_user_uid`| Override uid in the datasaker user. <br> (Default) `202306`|
|`datasaker_docker_user_gid`| Override gid in the datasaker user. <br> (Default) `202306`|
|`container_agent_image_tag`| Override the `dsk-container-agent` Image tag. <br> (Default) `latest`|
|`node_agent_image_tag`| Override the `dsk-node-agent` Image tag. <br> (Default) `latest`|
|`trace_agent_image_tag`| Override the `dsk-trace-agent` Image tag. <br> (Default) `latest`|
|`log_agent_image_tag`| Override the `dsk-log-agent` Image tag. <br> (Default) `latest`|
|`postgres_agent_image_tag`| Override the `dsk-postgres-agent` Image tag. <br> (Default) `latest`|
|`plan_postgres_agent_image_tag`| Override the `dsk-plan-postgres-agent` Image tag. <br> (Default) `latest`|-->

#### Agents Setting Role variables
| Variable                                   | Description                                      |
|--------------------------------------------|--------------------------------------------------|
|`trace_sampling_rate`| Override The `dsk-trace-agent` sampling rate applied to the collector.<br>- When set to 100 or higher, all data is collected.<br> (Default) `1`|
|`log_collects`|	An array of log collection configurations. Each item in the array includes the following. |
|`log_collects[*].paths`|	An array of paths for log collection. <br> (Default) [`host-agent`]=`/var/log/*.log`, [`docker-agent`]=`/var/log/sample/*/*.log` |
|`log_collects[*].exclude_paths`|	An array of paths to be excluded from the log collection. If the array is empty, no paths will be excluded. <br> (Default) `None` |
|`log_collects[*].keywords`|	An array of keywords for filtering the logs. If the array is empty, no keyword filtering will be applied. <br> (Default) `None` |
|`log_collects[*].tag`|	The tag for the log collection item. <br> (Default) `Default` |
|`log_collects[*].service.name`|	The name of the service. <br> (Default) `default` |
|`log_collects[*].service.category`|	The category of the service. <br> (Default) `etc` |
|`log_collects[*].service.type`|	The type of the service. <br> (Default) `etc` |
|`log_collects[*].service.address`|	The address of the service. <br> This field is optional and is used only for certain services that require an address. |
|`postgres_user_name`| Enter the Postgres user ID. <br> (Default) `None` |
|`postgres_user_password`| Enter the Postgres user password. <br> (Default) `None` |
|`postgres_database_address`| Enter the Postgres address. <br> (Default) `None` |
|`postgres_database_port`| Enter the Postgres port. <br> (Default) `None` |
|`plan_postgres_user_name`| Enter the Plan Postgres user ID. <br> (Default) `None` |
|`plan_postgres_user_password`| Enter the Plan Postgres user password. <br> (Default) `None` |
|`plan_postgres_database_address`| Enter the Plan Postgres address. <br> (Default) `None` |
|`plan_postgres_database_port`| Enter the Plan Postgres port. <br> (Default) `None` |
|`plan_postgres_database_name`| Enter the Plan Postgres database. <br> (Default) `None` |
|`plan_postgres_scrape_interval`| Override the Plan Postgres scrape interval <br> (Default) `30s` |
|`plan_postgres_scrape_timeout`| Override the Plan Postgres scrape timeout <br> (Default) `5s` |
|`plan_postgres_slow_query_standard`| Override the Plan Postgres slow query standard <br> (Default) `5s` |
|`plan_postgres_executor_number`| Override the Plan Postgres executor number <br> (Default) `10` |
|`plan_postgres_sender_number`| Override the Plan Postgres sender number <br> (Default) `10` |
|`plan_postgres_activity_query_buffer`| Override the Plan Postgres activity query buffer <br> (Default) `50` |
|`plan_postgres_plan_sender_buffer`| Override the Plan Postgres plan sender buffer <br> (Default) `50` |

###### Agent Variables Example
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

However for more control over the uninstall parameters, the following code can be used.
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
