# Ansible Datasaker Role

Ansibleを使用してDatasaker Agentをインストールできます。

## Requirements

- Ansible v2.6+が必要です。
- ほとんどのDebian Linuxディストリビューションをサポートします。
- Amazon Linux 2ディストリビューションをサポートします。

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

#### 必須設定

|変数名|説明
|--------------------------------------------|--------------------------------------------------- |
| `datasaker_api_key` | API Keyと入力します。
| `datasaker_agents` |各ホストにインストールするホストエージェントのリスト。 <br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>（Default） `dsk-node -agent`|
|`datasaker_docker_agents`|各ホストにインストールしたいDocker Container Agentのリスト。 Docker Container Agentsを挿入すると、Host Agentのインストールは自動的に無効になります。 <br>`dsk-docker-node-agent` `dsk-docker-trace-agent` `dsk-docker-log-agent` `dsk-docker-postgres-agent`<br>（Default） `dsk-docker- node-agent`|

<!--
#### Datasaker共通設定
|変数名|説明
|--------------------------------------------|--------------------------------------------------- |
|`datagate_url`| Datasaker Agent が送信する Datasaker Datagate URL の設定。 <br>（Default） `gate.jp.datasaker.io` |
|`datagate_trace_url`| `dsk-trace-agent` Datagate URL 設定。 <br>（Default） `datagate_url` |
|`datagate_trace_port`| `dsk-trace-agent` Datagate Portの設定。 <br>（Default） `31300` |
| `datagate_trace_timeout` | `dsk-trace-agent`データ転送の有効期限を設定します。 <br>(Default) `5s` |
|`datagate_manifest_url`| `dsk-manifest-agent` Datagate URL 設定。 <br>（Default） `datagate_url` |
|`datagate_manifest_port`| `dsk-manifest-agent` Datagate Portの設定。 <br>（Default） `31301` |
| `datagate_manifest_timeout` | `dsk-manifest-agent`データ転送の有効期限を設定します。 <br>(Default) `5s` |
|`datagate_metric_url`| `dsk-metric-agent` Datagate URL 設定。 <br>（Default） `datagate_url` |
|`datagate_metric_port`| `dsk-metric-agent` Datagate Portの設定。 <br>（Default） `31302` |
| `datagate_metric_timeout` | `dsk-metric-agent`データ転送の有効期限を設定します。 <br>(Default) `5s` |
|`datagate_plan_url`| `dsk-plan-agent` Datagate URLの設定。 <br>（Default） `datagate_url` |
|`datagate_plan_port`| `dsk-plan-agent` Datagate Portの設定。 <br>（Default） `31303` |
| `datagate_plan_timeout` | `dsk-plan-agent`データ転送の有効期限を設定します。 <br>(Default) `5s` |
|`datagate_loggate_url`| `dsk-log-agent` Datagate URL設定。 <br>（Default） `datagate_url` |
|`datagate_loggate_port`| `dsk-log-agent` Datagate Portの設定。 <br>（Default） `31304` |
| `datagate_loggate_timeout` | `dsk-log-agent`データ転送の有効期限を設定します。 <br>(Default) `5s` |
| `datasaker_api_url` | Datasaker API Server URL。 <br>（Default） `api.jp.datasaker.io` |
| `datasaker_api_send_interval` | Datasaker API Server Data 転送の有効期限を設定します。 <br>（Default） `1m` |
-->

#### Docker Container Agentの設定
|変数名|説明
|--------------------------------------------|--------------------------------------------------- |
| `datasaker_docker_config_path` | Datasaker Global Configの位置設定。 <br>(Default) `~/.datasaker` |
| `datasaker_docker_global_config` | Datasaker Global Config 名の設定。 <br> (Default) `~/.datasaker/config.yml` |
|`docker_default_path`| Datasaker Docker Log AgentにマウントするDocker Log収集の場所を設定します。 <br> (Default) `/var/lib/docker/containers/` |
|`datasaker_docker_path`| Datasaker Docker Agent Containerの場所を設定します。 <br> (Default) `/var/datasaker` |
|`container_agent_restart_policy`| `dsk-container-agent` Container Restart Policyの設定。 <br>（デフォルト） `always` |
|`node_agent_restart_policy`| `dsk-node-agent` Container Restart Policyの設定。 <br>（デフォルト） `always` |
|`trace_agent_restart_policy`| `dsk-trace-agent` Container Restart Policyの設定。 <br>（デフォルト） `always` |
|`log_agent_restart_policy`| `dsk-log-agent` Container Restart Policyの設定。 <br>（デフォルト） `always` |
|`postgres_agent_restart_policy`| `dsk-postgres-agent` Container Restart Policyの設定。 <br>（デフォルト） `always` |
| `plan_postgres_agent_restart_policy` |`dsk-plan-postgres-agent` Container Restart Policyの設定。 <br>（デフォルト） `always` |
|`container_agent_log_level`| `dsk-container-agent` Log Level設定。 <br>（Default） `INFO` |
|`node_agent_log_level`| `dsk-node-agent` Log Level設定。 <br>（Default） `INFO` |
|`trace_agent_log_level`| `dsk-trace-agent` Log Level設定。 <br>（Default） `INFO` |
|`log_agent_log_level`| `dsk-log-agent` Log Level設定。 <br>（Default） `INFO` |
|`postgres_agent_log_level`| `dsk-postgres-agent` Log Level設定。 <br>（Default） `INFO` |
|`plan_postgres_agent_log_level`| `dsk-plan-postgres-agent` Log Level設定。 <br>（Default） `INFO` |

<!--|`datasaker_docker_user`| Datasaker Docker Container Directory Ownership の設定。 <br>(Default) `datasaker` |
|`datasaker_docker_group`| Datasaker Docker Container Directory Groupの設定。 <br>(Default) `datasaker` |
|`datasaker_docker_user_uid`| Datasaker Docker Container Agent User UID 設定 <br> (Default) `202306` |
|`datasaker_docker_user_gid`| Datasaker Docker Container Agent User GID 設定 <br> (Default) `202306` |
|`container_agent_image_tag`| `dsk-container-agent` Image tag設定。 <br>（デフォルト） `latest` |
|`node_agent_image_tag`| `dsk-node-agent` Image tag設定。 <br>（デフォルト） `latest` |
|`trace_agent_image_tag`| `dsk-trace-agent` Image tag設定。 <br>（デフォルト） `latest` |
|`log_agent_image_tag`| `dsk-log-agent` Image tag設定。 <br>（デフォルト） `latest` |
|`postgres_agent_image_tag`| `dsk-postgres-agent` Image tag設定。 <br>（デフォルト） `latest` |
|`plan_postgres_agent_image_tag`| `dsk-plan-postgres-agent` Image tag設定。 <br> (Default) `latest`|-->

#### Datasaker Agentの詳細設定
- Host AgentとDocker Container Agentは同じ設定を使用します。

|変数名|説明
|--------------------------------------------|--------------------------------------------------- |
|`trace_sampling_rate`| `dsk-trace-agent`のcollectorに適用されるサンプリングレートを設定します。<br> - 100以上の場合、すべてのデータを収集します。<br>
|`log_collects`| `dsk-log-agent` のログ収集コレクション構成設定。リストの各項目には以下の項目が含まれます。 |
|`log_collects[*].paths`| `dsk-log-agent`でログを収集するためのパスを設定します。 <br> (Default) [`host-agent`]=`/var/log/*.log`, [`docker-agent`]=`/var/log/sample/*/*.log` |
|`log_collects[*].exclude_paths`| `dsk-log-agent` からログコレクションから除外するパスを設定します。値を指定しない場合は、収集パスに設定したすべてのログ収集。 <br>（デフォルト） `None` |
|`log_collects[*].keywords`| `dsk-log-agent`でログをフィルタリングするためのキーワード設定。値を指定しない場合は、すべてのログを収集します。 <br>（デフォルト） `None` |
|`log_collects[*].tag`| `dsk-log-agent`のユーザー設定タグ。 <br>（Default） `Default` |
|`log_collects[*].service.name`| `dsk-log-agent` で収集するサービス名の設定。 <br>（デフォルト） `default` |
|`log_collects[*].service.category`| `dsk-log-agent` によって収集されるサービス分類値の設定。 <br> `app` `database` `syslog` `etc`（デフォルト） `etc` |
|`log_collects[*].service.type`| `dsk-log-agent` で収集するサービスタイプの設定。 <br> `postgres` `mysql` `java` `etc`（デフォルト） `etc` |
|`log_collects[*].service.address`| `dsk-log-agent`が収集するサービスタイプがDatabaseの場合に作成されます。 - データベースのホストとポートについて
|`postgres_user_name`| `dsk-postgres-agent`にPostgresユーザーIDを設定します。 <br>（デフォルト） `None` |
| `postgres_user_password` | `dsk-postgres-agent`のPostgresユーザーパスワード設定。 <br>（デフォルト） `None` |
|`postgres_database_address`| `dsk-postgres-agent`にPostgres addressを設定します。 <br>（デフォルト） `None` |
|`postgres_database_port`| `dsk-postgres-agent`にPostgresポートを設定します。 <br>（デフォルト） `None` |
| `plan_postgres_user_name` | `dsk-plan-postgres-agent`にPlan PostgresユーザーIDを設定します。 <br>（デフォルト） `None` |
| `plan_postgres_user_password` | `dsk-plan-postgres-agent`のPlan Postgresユーザーパスワード設定。 <br>（デフォルト） `None` |
| `plan_postgres_database_address` | `dsk-plan-postgres-agent`にPlan Postgresアドレスを設定します。 <br>（デフォルト） `None` |
|`plan_postgres_database_port`| `dsk-plan-postgres-agent`にPlan Postgresポートを設定します。 <br>（デフォルト） `None` |
|`plan_postgres_database_name`| `dsk-plan-postgres-agent`にPlan Postgresデータベースを設定します。 <br>（デフォルト） `None` |
|`plan_postgres_scrape_interval`| `dsk-plan-postgres-agent`にPlan Postgresスクレープ間隔を設定します。 <br>（Default） `30s` |
| `plan_postgres_scrape_timeout` | `dsk-plan-postgres-agent`のPlan Postgresスクラップタイムアウト設定。 <br>（Default） `5s` |
|`plan_postgres_slow_query_standard`| `dsk-plan-postgres-agent`にPlan Postgres slow query standardを設定します。 <br>（Default） `5s` |
|`plan_postgres_executor_number`| `dsk-plan-postgres-agent` に Plan Postgres executor number を設定します。 <br>（Default） `10` |
|`plan_postgres_sender_number`| `dsk-plan-postgres-agent`にPlan Postgres sender numberを設定します。 <br>（Default） `10` |
| `plan_postgres_activity_query_buffer` | `dsk-plan-postgres-agent`にPlan Postgresアクティビティクエリバッファを設定します。 <br>（Default） `50` |
| `plan_postgres_plan_sender_buffer` | `dsk-plan-postgres-agent`にPlan Postgres plan sender bufferを設定します。 <br>（Default） `50` |

##### Ansible Playbook詳細設定 Example
``` yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents:
      - `dsk-docker-node-agent`
      - `dsk-docker-trace-agent`
      - `dsk-docker-log-agent`
      - `dsk-docker-postgres-agent`
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

Datasaker Agent を削除できます。
datasaker_clean は、uninstall を `True` に設定する必要があります。

|変数名|説明
|--------------------------------------------|--------------------------------------------------- |
|`uninstall`| `datasaker_agents` または `datasaker_docker_agents` に書かれたエージェントのみを削除します。 <br>（Default） `False` |
| `datasaker_clean` | `datasaker_agents` または `datasaker_docker_agents` に作成された Agent と、生成されたフォルダや設定ファイルまで削除。 <br>（Default） `False` |

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
<!--
## Troubleshooting

### Debian stretch

** 注：**
-->
