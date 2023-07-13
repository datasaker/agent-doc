# Ansible Datasaker Role

Ansibleを使用してDatasaker Agentをインストールできます。

## Requirements

* Ansible v2.6+が必要です。
*ほとんどのDebian Linuxディストリビューションをサポートしています。
* Amazon Linux 2ディストリビューションをサポートします。

## Installation

Ansible GalaxyからDatasaker roleをインストールします。

```shell
ansible-galaxy install dsk_bot.datasaker
```

エージェントを展開するためにAnsible playbookを作成します。

以下は基本的なインストールの例です。

#### Host Agent Default Install Example

``` yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent"]
```

#### Docker Agent Default Install Example

``` yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents: ["dsk-docker-node-agent","dsk-docker-log-agent"]
```

####必須設定

|変数名|説明
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `datasaker_api_key` | API Keyと入力します。 |
| `datasaker_agents` | <p>各ホストにインストールするホストエージェントのリスト。<br> <code> dsk-node-agent </code> <code> dsk-trace-agent </code> <code> dsk-log-agent < /code> <code>dsk-postgres-agent</code> <code>dsk-plan-postgres-agent</code><br>(Default) <code>dsk-node-agent</code></p> > |
| `datasaker_docker_agents` | <p>各ホストにインストールするDocker Container Agentのリスト。 Docker Container Agentsを挿入すると、Host Agentのインストールは自動的に無効になります。 docker-log-agent</code> <code>dsk-docker-postgres-agent</code><br>(Default) <code>dsk-docker-node-agent</code></p> |
| \<!-- | |
| #### Datasaker共通設定| |
|変数名|説明
| -------------------------------------------- | -------------------------------------------------- |
| `datagate_url` | <p>Datasaker Agentが送信するDatasaker Datagate URLの設定。<br>（Default）<code>gate.kr.datasaker.io </code> </p> |
| `datagate_trace_url` | <p><code>dsk-trace-agent</code> Datagate URL設定。 <br> (Default) <code>datagate_url</code></p> |
| `datagate_trace_port` | <p> <code> dsk-trace-agent </code> Datagate Portの設定。<br>（Default）<code> 31300 </code> </p> |
| `datagate_trace_timeout` | <p> <code> dsk-trace-agent </code>データ転送の有効期限を設定します。<br>（Default）<code> 5s </code> </p> |
| `datagate_manifest_url` | <p> <code> dsk-manifest-agent </code> Datagate URLの設定。<br>（Default）<code>datagate_url </code> </p> |
| `datagate_manifest_port` | <p> <code> dsk-manifest-agent </code> Datagate Portの設定。<br>（Default）<code> 31301 </code> </p> |
| `datagate_manifest_timeout` | <p> <code> dsk-manifest-agent </code>データ転送の有効期限を設定します。<br>（Default）<code> 5s </code> </p> |
| `datagate_metric_url` | <p> <code> dsk-metric-agent </code> Datagate URLの設定。<br>（Default）<code>datagate_url </code> </p> |
| `datagate_metric_port` | <p> <code> dsk-metric-agent </code> Datagate Portの設定。<br>（Default）<code> 31302 </code> </p> |
| `datagate_metric_timeout` | <p> <code> dsk-metric-agent </code>データ転送の有効期限を設定します。<br>（Default）<code> 5s </code> </p> |
| `datagate_plan_url` | <p> <code> dsk-plan-agent </code> Datagate URLの設定。<br>（Default）<code>datagate_url </code> </p> |
| `datagate_plan_port` | <p> <code> dsk-plan-agent </code> Datagate Portの設定。<br>（Default）<code> 31303 </code> </p> |
| `datagate_plan_timeout` | <p> <code> dsk-plan-agent </code>データ転送の有効期限を設定します。<br>（Default）<code> 5s </code> </p> |
| `datagate_loggate_url` | <p> <code> dsk-log-agent </code> Datagate URLの設定。<br>（Default）<code>datagate_url </code> </p>|
| `datagate_loggate_port` | <p> <code> dsk-log-agent </code> Datagate Portの設定。<br>（Default）<code> 31304 </code> </p> |
| `datagate_loggate_timeout` | <p> <code> dsk-log-agent </code>データ転送の有効期限を設定します。<br>（Default）<code> 5s </code> </p> |
| `datasaker_api_url` | <p>Datasaker API Server URL。<br>（Default）<code>api.kr.datasaker.io</code></p> |
| `datasaker_api_send_interval` | <p>Datasaker API Server Data転送の有効期限を設定します。<br>（Default）<code> 1m </code> </p> |
| --> | |

#### Docker Container Agentの設定

|変数名|説明
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| `datasaker_docker_config_path` | <p>Datasaker Global Configの場所を設定します。<br>（Default）<code>〜/.datasaker</code></p> |
| `datasaker_docker_global_config` | <p>Datasaker Global Configの名前を設定します。<br>(Default) <code>~/.datasaker/config.yml</code></p> |
| `docker_default_path` | <p>Datasaker Docker Log AgentにマウントするDocker Log収集の場所を設定します。
| `datasaker_docker_path` | <p>Datasaker Docker Agent Containerの場所を設定します。<br>（Default）<code> /var/datasaker </code> </p> |
| `container_agent_restart_policy` | <p> <code> dsk-container-agent </code> Container Restart Policyの設定。<br>(Default) <code>always</code></p> |
| `node_agent_restart_policy` | <p> <code> dsk-node-agent </code> Container Restart Policyの設定。<br>(Default) <code>always</code></p> |
| `trace_agent_restart_policy` | <p> <code> dsk-trace-agent </code> Container Restart Policy設定。<br>（Default）<code> always </code> </p> |
| `log_agent_restart_policy` | <p> <code> dsk-log-agent </code> Container Restart Policyの設定。<br>(Default) <code>always</code></p> |
| `postgres_agent_restart_policy` | <p> <code> dsk-postgres-agent </code> Container Restart Policyの設定。<br>(Default) <code>always</code></p> |
| `plan_postgres_agent_restart_policy` | <p> <code> dsk-plan-postgres-agent </code> Container Restart Policyの設定。<br>(Default) <code>always</code></p> |
| `container_agent_log_level` | <p> <code> dsk-container-agent </code> Log Level設定。<br>（Default）<code> INFO </code> </p> |
| `node_agent_log_level` | <p> <code> dsk-node-agent </code> Log Level設定。<br>（Default）<code> INFO </code> </p> |
| `trace_agent_log_level` | <p> <code> dsk-trace-agent </code> Log Level設定。<br>（Default）<code> INFO </code> </p> |
| `log_agent_log_level` | <p> <code> dsk-log-agent </code> Log Level設定。<br>（Default）<code> INFO </code> </p> |
| `postgres_agent_log_level` | <p><code>dsk-postgres-agent </code> Log Level設定。<br>（Default）<code> INFO </code> </p> |
| `plan_postgres_agent_log_level` | <p> <code> dsk-plan-postgres-agent </code> Log Level設定。<br>（Default）<code> INFO </code> </p> |

#### Datasaker Agentの詳細設定

* Host AgentとDocker Container Agentは同じ設定値を使用します。

|変数名|説明
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `trace_sampling_rate` | <p> <code> dsk-trace-agent </code>でcollectorに適用されるサンプリング率を設定します。<br> - 100以上の場合にすべてのデータを収集します。 </p> |
| `log_collects` | `dsk-log-agent` のログ収集コレクション構成設定。リストの各項目には以下の項目が含まれます。 |
| `log_collects[*].paths` | <p> <code> dsk-log-agent </code>でログを収集するためのパスを設定します。<br>(Default) [<code>host-agent</code>]=<code>/var/log/ *.log</code>、[<code>docker-agent</code>]=<code>/var/log/sample/*/*.log</code></p> |
| `log_collects[*].exclude_paths` | <p> <code> dsk-log-agent </code>からログコレクションから除外するパスを設定します。値を指定しない場合は、収集パスに設定したすべてのログを収集します。<br>（Default）<code> None </code> </p> |
| `log_collects[*].keywords` | <p> <code> dsk-log-agent </code>でログをフィルタリングするためのキーワード設定。値を指定しないと、すべてのログが収集されます。<br>（Default）<code> None </code> </p> |
| `log_collects[*].tag` | <p> <code> dsk-log-agent </code>のカスタムタグ。<br>（Default）<code>Default</code> </p> |
| `log_collects[*].service.name` | <p> <code> dsk-log-agent </code>が収集するサービス名を設定します。<br>（Default）<code> default </code> </p> |
| `log_collects[*].service.category` | <p> <code> dsk-log-agent </code>によって収集されるサービス分類値の設定。<br><code>app</code> <code>database</code> <code>syslog</code> <code>etc</code> (Default) <code>etc</code></p> |
| `log_collects[*].service.type` | <p> <code> dsk-log-agent </code>によって収集されるサービスタイプの設定。<br><code>postgres</code> <code>mysql</code> <code>java</code> < code>etc</code> (Default) <code>etc</code></p> |
| `log_collects[*].service.address` | <p> <code> dsk-log-agent </code>が収集するサービスタイプがDatabaseの場合に作成<br> - データベースのホストとポートについて</p> |
| `postgres_user_name` | <p> <code> dsk-postgres-agent </code>にPostgres user IDを設定します。<br>（Default）<code> None </code> </p> |
| `postgres_user_password` | <p> <code> dsk-postgres-agent </code>でPostgresユーザーパスワードを設定します。<br>（Default）<code> None </code> </p> |
| `postgres_database_address` | <p> <code> dsk-postgres-agent </code>にPostgres addressを設定します。<br>（Default）<code> None </code> </p> |
| `postgres_database_port` | <p> <code> dsk-postgres-agent </code>でPostgresポートを設定します。<br>（Default）<code> None </code> </p> |
| `plan_postgres_user_name` | <p> <code> dsk-plan-postgres-agent </code>にPlan PostgresユーザーIDを設定します。<br>（Default）<code> None </code> </p>
| `plan_postgres_user_password` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgresユーザーパスワードを設定します。<br>（Default）<code> None </code> </p> |
| `plan_postgres_database_address` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgres addressを設定します。<br>（Default）<code> None </code> </p> |
| `plan_postgres_database_port` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgresポートを設定します。<br>（Default）<code> None </code> </p> |
| `plan_postgres_database_name` | <p> <code> dsk-plan-postgres-agent</code>でPlan Postgresデータベースを設定します。<br>（Default）<code> None </code> </p> |
| `plan_postgres_scrape_interval` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgres scrape intervalを設定します。<br>（Default）<code> 30s </code> </p> |
| `plan_postgres_scrape_timeout` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgresスクラップタイムアウトを設定します。<br>（Default）<code> 5s </code> </p> |
| `plan_postgres_slow_query_standard` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgres slow query standardを設定します。<br>（Default）<code> 5s </code> </p> |
| `plan_postgres_executor_number` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgres executor numberを設定します。<br>（Default）<code> 10 </code> </p> |
| `plan_postgres_sender_number` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgres sender numberを設定します。<br>（Default）<code> 10 </code> </p> |
| `plan_postgres_activity_query_buffer` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgres activity query bufferを設定します。<br>（Default）<code> 50 </code> </p> |
| `plan_postgres_plan_sender_buffer` | <p> <code> dsk-plan-postgres-agent </code>でPlan Postgres plan sender bufferを設定します。<br>（Default）<code> 50 </code> </p> |

**Ansible Playbook詳細設定Example**

``` yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents:
      - ```dsk-docker-node-agent
      - ```dsk-docker-trace-agent
      - ```dsk-docker-log-agent
      - ```dsk-docker-postgres-agent
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
          category: "データベース"
          type: "postgres"
          address: "0.0.0.0:5432"
```

## Uninstallation

Datasaker Agent を削除できます。 datasaker\_clean は、uninstall を `True` に設定する必要があります。

|変数名|説明
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `uninstall` | <p><code>datasaker_agents</code>または<code>datasaker_docker_agents</code>で作成されたエージェントのみを削除します。<br>（Default）<code> False </code> </p> |
| `datasaker_clean` | <p><code>datasaker_agents</code>または<code>datasaker_docker_agents</code>で作成されたAgentと作成されたフォルダと設定ファイルまで削除します。<br>（Default）<code>False</code> > |

#### Datasaker Agents Uninstall Example

``` yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_agents: ["<AGENT_NAME>"]
    uninstall: True
    datasaker_clean: True
```