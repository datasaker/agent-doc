# dsk-log-agent

## Docker環境にDatasaker Log agentをインストールする

`Log agent`は、さまざまな環境でシステムまたはアプリケーションによって生成されたログデータをほぼリアルタイムで収集、処理、転送するエージェントです。 `Log agent`を介して複数のサーバーから生成されたログデータを集中的に管理および分析するために使用できます。これにより、ユーザーはシステムまたはアプリケーションで発生する問題をすばやく検出して対応できます。さらに、ログデータを分析することで、セキュリティ、パフォーマンス、ビジネス分析など、さまざまな目的に活用できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在Docker環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (dsk-log-agent/ja/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Log agentのインストール

エージェントを介してログを収集するには、ユーザーが収集したいログパスをエージェントにマウントする必要があります。

### 1. エージェントの実行に必要な構成 YAML ファイルを生成します。

エージェントにログボリュームをマウントしたパスに基づいてエージェント構成YAMLファイルを作成します。

ユーザーが収集したいログパス（例： `/var/lib/docker/containers/`）をエージェントの特定のパス（たとえば `/var/log/sample/`）にマウントする場合、次のように設定ファイルを書くことができます。

```shell
cat << EOF> ~/.datasaker/log-agent-config.yml
agent:
  metadata:
    agent_name: 'dsk-log-agent'
  collect:
    - paths:
        - '/var/log/sample/*.log'
      exclude_paths:
        - '/var/log/sample/private.log'
      keywords:
        - ```ERROR
        - ```WARN
      tag: 'Sample Service'
      service:
        name: 'Sample'
        category: 'database'
        type: 'postgres'
        address: 'my-sample-serivce:5432'
EOF
```

**\[注意]** `agent.collect.paths[]`設定項目は、マウントされたボリュームパスに基づいて作成する必要があります（例： '/var/log/sample/'）場合、ログエージェントが正常に動作しない可能性があります。

ログエージェント構成ファイル内の各設定項目の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Necessary** |
| -------------------------------- | --------------------------------------------------------------------------------------- | :-------------: | :-----------: |
| `agent.metadata.agent_name` |ログエージェント名| `dsk-log-agent` | |
| `agent.collect.paths[]` |ログ収集先のパス（例：/var/log/sample/\*.log） N / A | **✓** |
| `agent.collect.exclude_paths[]` |ログ収集除外ターゲットパスN / A | |
| `agent.collect.keywords[]` |ログ収集キーワード（キーワードを含むログのみを収集します。）| N / A | |
| `agent.collect.tag` |カスタムタグ| N / A | |
| `agent.collect.service.name` |サービス名| `default` | |
| `agent.collect.service.category` |サービス分類（ `app`、`database`、`syslog`、`etc`のいずれかの値を作成してください。）| `etc` | |
| `agent.collect.service.type` |サービスデータベースの種類と開発言語の種類（ `postgres`、`mysql`、`java`、`etc`のいずれかの値を書いてください） `etc` | |
| `agent.collect.service.address` |データベースのホストとポートについて（サービスの分類がデータベースの場合は設定してください。設定しないと、特定の機能を使用できない可能性があります。） N / A | ⚠️|

### 2. docker コマンドでログエージェントを実行します。

*ログエージェントに必要な設定ファイルをマウントします。 (global, agent YAML configiuration files)
  * `-v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro`
  * `-v ~/.datasaker/log-agent-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml:ro`
  * **\[注意]** global および agent 構成ファイルは必ず作成してください。作成しないと、ログエージェントが正常に動作しない可能性があります。
*ユーザーが収集したいログをエージェントにマウントします。
  *次のようにオプションを設定します。 `-v [COLLECT LOG PATH]:[LOG AGENT MOUNT PATH]:ro`
    *そのオプションでは、```：の前のパスはログを収集するパスを作成し、```：の後のパスはログを収集するエージェントのパスを作成します。
    * `[LOG AGENT MOUNT PATH]`パスに基づいてエージェント構成ファイルのログ収集パスを作成する必要があります。
* ログファイルをマウントする場合は、必ず `-mount.volume=true` オプションを設定してください。

以下は、ログエージェントの実行例です。

```shell
docker run -d --name dsk-log-agent\
  -v /var/datasaker/:/var/datasaker/\
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro \
  -v ~/.datasaker/log-agent-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml:ro \
  -v /var/lib/docker/containers/:/var/log/sample/:ro\
  --restart=always\
  datasaker/dsk-log-agent:latest \
  -mount.volume=true
```

## Log agentの使用例

ログエージェント設定ファイルを適切に使用して、さまざまな方法で収集設定を行うことができます。

*異なるパスのログを1つのサービス名で収集できます。 （ `paths`エントリに複数のログファイルパスを作成してください。）
*異なるサービスス `category`のログファイルを1つのサービス名で収集できます。 （別々の `collect`を書いた後、 `service.name`エントリを同じように書いてください。）
*特定のパスにあるすべてのログファイルを収集できます（```*を使用してログ収集パスを設定します）。また、そのパスから除外したいログファイルがある場合は、```exclude_pathsエントリに書き込みます。

以下は、ログ収集設定の例です。

`` yaml
agent:
  metadata:
    agent_name: 'Apple & Banana Service Log Agent'
  collect:
    - paths:
        - '~/datasaker/log/apple/app/*.log'
      keywords:
        - ```400
        - '500'
      service:
        name: 'Apple'
        category: 'app'
        type: 'java'
    - paths:
        - '~/datasaker/log/apple/database/*.log'
      keywords:
        - ```ERROR
      service:
        name: 'Apple'
        category: 'database'
        type: 'postgres'
        address: 'apple-serivce:5432'
    - paths:
        - '~/datasaker/log/banana/app/*.log'
      keywords:
        - ```ERROR
      service:
        name: 'Banana'
        category: 'app'
        type: 'etc'
```

```shell
docker run -d --name dsk-log-agent\
  -v /var/datasaker/:/var/datasaker/\
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro \
  -v ~/.datasaker/log-agent-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml:ro \
  -v ~/var/lib/docker/containers/APPLE-APP-SERVER:~/datasaker/apple/app/:ro \
  -v ~/var/lib/docker/containers/APPLE-DB-SERVER:~/datasaker/apple/database/:ro \
  -v ~/var/lib/docker/containers/BANANA-APP-SERVER:~/datasaker/banana/app/:ro \
  --restart=always\
  datasaker/dsk-log-agent:latest \
  -mount.volume=true
```