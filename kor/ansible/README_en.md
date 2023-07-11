# Ansible Datasaker Role

The Ansible Datasaker role installs and configures the Datasaker Agent and integrations.

## Setup

### Requirements

* Requires Ansible v2.6+.
* Supports most Debian Linux distributions.
* Supports Amazon Linux 2 distributions.

### Installation

Install the \[Datasaker role] from Ansible Galaxy on your Ansible server:

```shell
ansible-galaxy install dsk_bot.datasaker
```

To deploy the Datasaker Agent on hosts, add the Datasaker role and your API key to your playbook:

**Host Agent Default Install Example**

```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent"] 
```

**Docker Agent Default Install Example**

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

| Variable                                     | Description                                                                                                                                                                                                                              |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `datasaker_api_key`                          | Your Datasaker API key.                                                                                                                                                                                                                  |
| `datasaker_agents`                           | <p>Set to Datasaker Host Agent.<br><code>dsk-node-agent</code> <code>dsk-trace-agent</code> <code>dsk-log-agent</code> <code>dsk-postgres-agent</code> <code>dsk-plan-postgres-agent</code><br>(Default) <code>dsk-node-agent</code></p> |
| `datasaker_docker_agents`                    | <p>Set to Datasaker Docker Agent.<br><code>dsk-docker-node-agent</code> <code>dsk-docker-trace-agent</code> <code>dsk-docker-log-agent</code> <code>dsk-docker-postgres-agent</code><br>(Default) <code>dsk-docker-node-agent</code></p> |
| \<!--                                        |                                                                                                                                                                                                                                          |
| #### Agent Global Config Role variables      |                                                                                                                                                                                                                                          |
| Variable                                     | Description                                                                                                                                                                                                                              |
| -------------------------------------------- | --------------------------------------------------                                                                                                                                                                                       |
| `datagate_url`                               | <p>The site of the Datasaker intake to send Agent data to.<br>(Default) <code>gate.kr.datasaker.io</code></p>                                                                                                                            |
| `datagate_trace_url`                         | <p>Override the <code>dsk-trace-agent</code> datagate url.<br>(Default) <code>datagate_url</code></p>                                                                                                                                    |
| `datagate_trace_port`                        | <p>Override the <code>dsk-trace-agent</code> datagate port.<br>(Default) <code>31300</code></p>                                                                                                                                          |
| `datagate_trace_timeout`                     | <p>Override the <code>dsk-trace-agent</code> data expiration time.<br>(Default) <code>5s</code></p>                                                                                                                                      |
| `datagate_manifest_url`                      | <p>Override the <code>dsk-manifest-agent</code> datagate url.<br>(Default) <code>datagate_url</code></p>                                                                                                                                 |
| `datagate_manifest_port`                     | <p>Override the <code>dsk-manifest-agent</code> datagate port.<br>(Default) <code>31301</code></p>                                                                                                                                       |
| `datagate_manifest_timeout`                  | <p>Override the <code>dsk-manifest-agent</code> data expiration time.<br>(Default) <code>5s</code></p>                                                                                                                                   |
| `datagate_metric_url`                        | <p>Override the <code>dsk-metric-agent</code> datagate url.<br>(Default) <code>datagate_url</code></p>                                                                                                                                   |
| `datagate_metric_port`                       | <p>Override the <code>dsk-metric-agent</code> datagate port.<br>(Default) <code>31302</code></p>                                                                                                                                         |
| `datagate_metric_timeout`                    | <p>Override the <code>dsk-metric-agent</code> data expiration time.<br>(Default) <code>5s</code></p>                                                                                                                                     |
| `datagate_plan_url`                          | <p>Override the <code>dsk-plan-agent</code> datagate url.<br>(Default) <code>datagate_url</code></p>                                                                                                                                     |
| `datagate_plan_port`                         | <p>Override the <code>dsk-plan-agent</code> datagate port.<br>(Default) <code>31303</code></p>                                                                                                                                           |
| `datagate_plan_timeout`                      | <p>Override the <code>dsk-plan-agent</code> data expiration time.<br>(Default) <code>5s</code></p>                                                                                                                                       |
| `datagate_loggate_url`                       | <p>Override the <code>dsk-log-agent</code> datagate url.<br>(Default) <code>datagate_url</code></p>                                                                                                                                      |
| `datagate_loggate_port`                      | <p>Override the <code>dsk-log-agent</code> datagate port.<br>(Default) <code>31304</code></p>                                                                                                                                            |
| `datagate_loggate_timeout`                   | <p>Override the <code>dsk-log-agent</code> data expiration time.<br>(Default) <code>5s</code></p>                                                                                                                                        |
| `datasaker_api_url`                          | <p>Override the datasaker api server url.<br>(Default) <code>api.kr.datasaker.io</code></p>                                                                                                                                              |
| `datasaker_api_send_interval`                | <p>Override the datasaker api server data expiration time.<br>(Default) <code>1m</code></p>                                                                                                                                              |
| -->                                          |                                                                                                                                                                                                                                          |

#### Docker Agent Role variables

| Variable                             | Description                                                                                                              |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `datasaker_docker_config_path`       | <p>Override the datasaker global config path.<br>(Default) <code>~/.datasaker</code></p>                                 |
| `datasaker_docker_global_config`     | <p>Override the datasaker global config file name.<br>(Default) <code>~/.datasaker/config.yml</code></p>                 |
| `docker_default_path`                | <p>Override the docker containers path.<br>(Default) <code>/var/lib/docker/containers/</code></p>                        |
| `datasaker_docker_path`              | <p>Override the datasaker docker agent containers path.<br>(Default) <code>/var/datasaker</code></p>                     |
| `container_agent_restart_policy`     | <p>Override the restart policy for a <code>dsk-container-agent</code> container<br>(Default) <code>always</code></p>     |
| `node_agent_restart_policy`          | <p>Override the restart policy for a <code>dsk-node-agent</code> container<br>(Default) <code>always</code></p>          |
| `trace_agent_restart_policy`         | <p>Override the restart policy for a <code>dsk-trace-agent</code> container<br>(Default) <code>always</code></p>         |
| `log_agent_restart_policy`           | <p>Override the restart policy for a <code>dsk-log-agent</code> container<br>(Default) <code>always</code></p>           |
| `postgres_agent_restart_policy`      | <p>Override the restart policy for a <code>dsk-postgres-agent</code> container<br>(Default) <code>always</code></p>      |
| `plan_postgres_agent_restart_policy` | <p>Override the restart policy for a <code>dsk-plan-postgres-agent</code> container<br>(Default) <code>always</code></p> |
| `container_agent_log_level`          | <p>Override the <code>dsk-container-agent</code> log level<br>(Default) <code>INFO</code></p>                            |
| `node_agent_log_level`               | <p>Override the <code>dsk-node-agent</code> log level<br>(Default) <code>INFO</code></p>                                 |
| `trace_agent_log_level`              | <p>Override the <code>dsk-trace-agent</code> log level<br>(Default) <code>INFO</code></p>                                |
| `log_agent_log_level`                | <p>Override the <code>dsk-log-agent</code> log level<br>(Default) <code>INFO</code></p>                                  |
| `postgres_agent_log_level`           | <p>Override the <code>dsk-postgres-agent</code> log level<br>(Default) <code>INFO</code></p>                             |
| `plan_postgres_agent_log_level`      | <p>Override the <code>dsk-plan-postgres-agent</code> log level<br>(Default) <code>INFO</code></p>                        |

#### Agents Setting Role variables

| Variable                              | Description                                                                                                                                                                       |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `trace_sampling_rate`                 | <p>Override The <code>dsk-trace-agent</code> sampling rate applied to the collector.<br>- When set to 100 or higher, all data is collected.<br>(Default) <code>1</code></p>       |
| `log_collects`                        | An array of log collection configurations. Each item in the array includes the following.                                                                                         |
| `log_collects[*].paths`               | <p>An array of paths for log collection.<br>(Default) [<code>host-agent</code>]=<code>/var/log/*.log</code>, [<code>docker-agent</code>]=<code>/var/log/sample/*/*.log</code></p> |
| `log_collects[*].exclude_paths`       | <p>An array of paths to be excluded from the log collection. If the array is empty, no paths will be excluded.<br>(Default) <code>None</code></p>                                 |
| `log_collects[*].keywords`            | <p>An array of keywords for filtering the logs. If the array is empty, no keyword filtering will be applied.<br>(Default) <code>None</code></p>                                   |
| `log_collects[*].tag`                 | <p>The tag for the log collection item.<br>(Default) <code>Default</code></p>                                                                                                     |
| `log_collects[*].service.name`        | <p>The name of the service.<br>(Default) <code>default</code></p>                                                                                                                 |
| `log_collects[*].service.category`    | <p>The category of the service.<br>(Default) <code>etc</code></p>                                                                                                                 |
| `log_collects[*].service.type`        | <p>The type of the service.<br>(Default) <code>etc</code></p>                                                                                                                     |
| `log_collects[*].service.address`     | <p>The address of the service.<br>This field is optional and is used only for certain services that require an address.</p>                                                       |
| `postgres_user_name`                  | <p>Enter the Postgres user ID.<br>(Default) <code>None</code></p>                                                                                                                 |
| `postgres_user_password`              | <p>Enter the Postgres user password.<br>(Default) <code>None</code></p>                                                                                                           |
| `postgres_database_address`           | <p>Enter the Postgres address.<br>(Default) <code>None</code></p>                                                                                                                 |
| `postgres_database_port`              | <p>Enter the Postgres port.<br>(Default) <code>None</code></p>                                                                                                                    |
| `plan_postgres_user_name`             | <p>Enter the Plan Postgres user ID.<br>(Default) <code>None</code></p>                                                                                                            |
| `plan_postgres_user_password`         | <p>Enter the Plan Postgres user password.<br>(Default) <code>None</code></p>                                                                                                      |
| `plan_postgres_database_address`      | <p>Enter the Plan Postgres address.<br>(Default) <code>None</code></p>                                                                                                            |
| `plan_postgres_database_port`         | <p>Enter the Plan Postgres port.<br>(Default) <code>None</code></p>                                                                                                               |
| `plan_postgres_database_name`         | <p>Enter the Plan Postgres database.<br>(Default) <code>None</code></p>                                                                                                           |
| `plan_postgres_scrape_interval`       | <p>Override the Plan Postgres scrape interval<br>(Default) <code>30s</code></p>                                                                                                   |
| `plan_postgres_scrape_timeout`        | <p>Override the Plan Postgres scrape timeout<br>(Default) <code>5s</code></p>                                                                                                     |
| `plan_postgres_slow_query_standard`   | <p>Override the Plan Postgres slow query standard<br>(Default) <code>5s</code></p>                                                                                                |
| `plan_postgres_executor_number`       | <p>Override the Plan Postgres executor number<br>(Default) <code>10</code></p>                                                                                                    |
| `plan_postgres_sender_number`         | <p>Override the Plan Postgres sender number<br>(Default) <code>10</code></p>                                                                                                      |
| `plan_postgres_activity_query_buffer` | <p>Override the Plan Postgres activity query buffer<br>(Default) <code>50</code></p>                                                                                              |
| `plan_postgres_plan_sender_buffer`    | <p>Override the Plan Postgres plan sender buffer<br>(Default) <code>50</code></p>                                                                                                 |

**Agent Variables Example**

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

However for more control over the uninstall parameters, the following code can be used. In this example:

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
