# dsk-log-agent

## Docker環境にDatasaker Log agentをインストールする

`Log agent`は、さまざまな環境でシステムまたはアプリケーションによって生成されたログデータをほぼリアルタイムで収集、処理、転送するエージェントです。 `Log agent`を介して複数のサーバーから生成されたログデータを集中的に管理および分析するために使用できます。これにより、ユーザーはシステムまたはアプリケーションで発生する問題をすばやく検出して対応できます。さらに、ログデータを分析することで、セキュリティ、パフォーマンス、ビジネス分析など、さまざまな目的に活用できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在、Docker環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (dsk-log-agent/ja/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Log agentのインストール

Docker環境でログエージェントを介してログを収集するには、次の作業が必要です。

### 1. エージェントの実行に必要な構成 YAML ファイルを生成します。

ログエージェントは、エージェント設定YAMLファイルを介してログ収集設定を管理します。ログエージェント構成ファイル内の各設定項目の説明は次のとおりです。
```yaml
agent:
  metadata:
    agent_name:
    cluster_id:
  logs:
    - service:
      tag: []
      keyword: []
      multiline:
        format:
        pattern: []
      masking:
        - pattern:
          replace:
      collect:
        type:
        category:
        address:
        file:
          paths: []
          exclude_paths: []
        driver:
          containers: []
```
| **Settings** | **Description** | **Default** |
| ----------------------------------- | -------------------------------------------------------------------------------- |:----------------:|
| **metadata** |エージェント基本情報|
| `agent_name` |ログエージェント名| `dsk-log-agent` |
| `cluster_id` |管理対象となる環境のクラスタ情報`unknown` |
| **logs** |ログ収集対象情報|
| `service` |ログ収集先のサービス名`default` |
| `tag` |ログ収集対象のタグN / A |
| `keyword` |ログ収集キーワード（キーワードを含むログのみを収集）| |
| **multiline** |マルチラインログ収集設定| |
| `format` |マルチラインログフォーマット（例：go、java、ruby、python）| |
| `pattern` |マルチラインログパターン（例：^\d{4}-\d{2}-\d{2}） - ユーザーカスタム正規表現パターンが利用可能です。 |
| **masking** |機密情報ログマスキング設定| |
| `pattern` |マスクするログパターン（たとえば、^\d{4}-\d{2}-\d{2}） - ユーザーカスタム正規表現パターンを使用可能|
| `replace` |マスキングパターンを置き換える文字列（例：*****） |
| **collect** |ログ収集先設定| |
| `type` |ログ収集方法（ `file`、`driver`のいずれかの値を作成する）| `file` |
| `カテゴリー`|サービス分類（ `app`、`database`、`syslog`、`etc`のいずれかの値を作成する） `etc` |
| `address` |データベースホストとポート情報（サービス分類がデータベースの場合は設定） |
| **file** |ログ収集方法がファイルの場合|
| `paths` |ログ収集先のパス（例：/var/log/sample/*.log） `/var/log/*.log` |
| `exclude_paths` |ログ収集除外ターゲットパス|
| **driver** |ログ収集方法がドライバの場合|
| `containers` |ログ収集先のコンテナ名`*` |

Docker環境では、ログエージェントは2つの方法でログを収集できます。

***driver**: ドッカーロギングドライバを使用してログを収集します。収集するコンテナ名を設定します。 （標準出力でログが出力される場合は、その方法を使用してください。）
***file**: ログファイルを直接収集します。収集するログをログエージェントに直接マウントし、収集ログファイルの相対パスを設定します。 （標準出力でログが出力されない場合は、その方法を使用してください。）たとえば、ロギングドライバを介してコンテナ名がawesome_sakerのログを収集する場合は、次のように設定ファイルを作成できます。
```shell
cat << EOF > ~/.datasaker/log-agent-config.yml
agent:
  logs:
    - collect:
        type: driver
        driver:
          containers:
           - awesome_saker
EOF
```
ログファイルを直接収集する場合は、次のように構成ファイルを作成できます。
```shell
cat << EOF > ~/.datasaker/log-agent-config.yml
agent:
  logs:
    - collect:
        type: file
        file:
          paths:
           - /var/lib/docekr/containers/*awesome_saker*.log
EOF
```
### 2. docker コマンドでログエージェントを実行します。

*（必須）ログエージェントに必要な設定ファイルをマウントします。 (global, agent YAML configiuration files)
  * `-v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro`
  * `-v ~/.datasaker/log-agent-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml:ro`
  * **\[注意]** global および agent 構成ファイルは必ず作成してください。作成しないと、ログエージェントが正常に動作しない可能性があります。
* (ロギングドライバを介して収集する場合) ポート転送設定が必要です。
  * `-p [HOST_PORT_NUMBER]:21212`
*（ログファイルを直接収集する場合）ユーザーが収集したいログをエージェントにマウントします。
  * 次のようにオプションを設定します。 `-v [COLLECT LOG PATH]:[LOG AGENT MOUNT PATH]:ro`
    * そのオプションでは、`：`の前のパスはログを収集するパスを作成し、`：`の後のパスはログを収集するエージェントのパスを作成します。
    * `[LOG AGENT MOUNT PATH]`パスに基づいてエージェント構成ファイルのログ収集パスを作成する必要があります。

以下は、ログエージェントの実行例です。
```shell
docker  run -d --name dsk-log-agent \
  -v /var/datasaker/:/var/datasaker/ \
  -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro \
  -v ~/.datasaker/log-agent-config.yml:/etc/datasaker/dsk-log-agent/agent-config.yml:ro \
  -v -v /var/lib/docker/containers/:/var/lib/docker/containers/:ro \
  -p 21212:21212 \
  --restart=always \
  datasaker/dsk-log-agent:latest
```
**\[注意]** ログファイルを直接収集する場合は、そのログをエージェントにマウントする必要があります。

### 3.（ログ記録ドライバを介して収集する場合）ログを収集するターゲットのコンテナを実行します。

*ログ収集に必要なオプションを設定します。
  * `--log-driver=fluentd`
  * `--log-opt fluentd-address=[LOG_AGENT_HOST]:[LOG_AGENT_HOST_PORT_NUMBER]`

**\[注意]** ロギングドライバを介してログを収集する場合は、ログを収集する対象のコンテナを再実行する必要があります。

以下は、ログ収集のためのコンテナ実行の例です。
```shell
docker  run --log-driver=fluentd --log-opt fluentd-address=dsk-log-agent:21212 your/application
```
