# dsk-log-agent

`Log agent`は、さまざまな環境でシステムまたはアプリケーションによって生成されたログデータをほぼリアルタイムで収集、処理、転送するエージェントです。 `Log agent`を介して複数のサーバーから生成されたログデータを集中的に管理および分析するために使用できます。これにより、ユーザーはシステムまたはアプリケーションで発生する問題をすばやく検出して対応できます。さらに、ログデータを分析することで、セキュリティ、パフォーマンス、ビジネス分析など、さまざまな目的に活用できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在、Amazon Linux 2023環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (README.md)

## Log agentのインストール

### fluent-bitパッケージのインストール

`Log agent`はfluent-bitを介してログを収集し、エージェントのインストール時にfluent-bit 2.1.0以降をインストールする必要があります。

次のコマンドでfluent-bit 2.1.0以降をインストールします。
```shell
curl https://raw.githubusercontent.com/fluent/fluent-bit/master/install.sh | sh
```
fluent-bitパッケージのインストールの詳細については、`公式文書`（https://docs.fluentbit.io/manual/installation/linux/amazon-linux）を参照してください。

### 1. パッケージのインストール
```shell
yum install dsk-log-agent
```
### 2. agent-configの設定

`/etc/datasaker/dsk-log-agent/agent-config.yml`パスにagent-configファイルを作成します。

たとえば、ログファイルを収集する場合は、次のように設定ファイルを作成できます。
```yaml
agent:
  logs:
    - collect:
        type: file
        file:
          paths:
            - '/var/log/sample/*.log'
```
>注意：ログファイル以外のファイルがログ収集先のパスに設定されないように設定してください。ログ収集パスのファイルを指定するか、それ以外のファイルを除外します。ログ収集設定にファイル拡張子形式を必ず作成してください。 （例： `.log`）

### 3. パッケージの実行
```shell
systemctl enable dsk-log-agent --now
```
### 4. パッケージ実行状態の確認
```shell
systemctl status dsk-log-agent
```
## Log agentを削除する

### 1. パッケージの中断
```shell
systemctl stop dsk-log-agent
```
### 2. パッケージの削除

パッケージを削除する前に、必ずパッケージを中断して削除してください。
```shell
yum remove dsk-log-agent
```
## Log agent 構成の設定

ログエージェントのログ収集設定を変更したい場合は、 `/etc/datasaker/dsk-log-agent/agent-config.yml`パスで設定ファイルを変更してください。

以下は、ログエージェント構成ファイル内の各設定項目の説明です。
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
```
| **Settings** | **Description** | **Default** |
| ----------------------------------- | --------------------------------------------------------------------------------------- |:-----------------:|
| **metadata** |エージェント基本情報
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
| **file** |ログ収集方法がfileの場合|
| `paths` |ログ収集先のパス（例：/var/log/sample/*.log） `/var/log/*.log` |
| `exclude_paths` |ログ収集除外ターゲットパス|