# dsk-log-agent

## Kubernetes環境でDataSaker Log agentをインストールする

`Log agent`は、さまざまな環境でシステムまたはアプリケーションによって生成されたログデータをほぼリアルタイムで収集、処理、転送するエージェントです。 `Log agent`を介して複数のサーバーから生成されたログデータを集中的に管理および分析するために使用できます。これにより、ユーザーはシステムまたはアプリケーションで発生する問題をすばやく検出して対応できます。さらに、ログデータを分析することで、セキュリティ、パフォーマンス、ビジネス分析など、さまざまな目的に活用できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (dsk-log-agent/ja/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Log agentのインストール

デフォルトでは、 `Log agent`はクーバネティス環境でデフォルトでデーモンセットとして配布されます。したがって、すべてのノードに `Log agent` がインストールされます。特定のノードにのみLog agentをインストールしたい場合は、そのノードにaffinityまたはnodeSelectorを追加設定してください。

### 1. Log agent 設定値の登録

`Log agent`の設定値の意味とデフォルト設定値は次のとおりです。ユーザーごとにエージェント設定に異なる要件があります。したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。最適な結果を得るためにエージェント設定を調整してください。 `\~/datasaker/config.yaml`でその値を追加または変更します。

以下は、ログエージェント構成ファイル内の各設定項目の説明です。
```yaml
logAgent:
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
        kubernetes:
          - namespace:
            pod:
            container:
```
| **Settings** | **Description** | **Default** |
| ----------------------------------- | --------------------------------------------------------------------------------------- |:-----------------:|
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
| `type` |ログ収集方法（ `file`、`driver`、`kubernetes`のいずれかの値を作成する）| `file` |
| `カテゴリー`|サービス分類（ `app`、`database`、`syslog`、`etc`のいずれかの値を作成する） `etc` |
| `address` |データベースホストとポート情報（サービス分類がデータベースの場合は設定） |
| **file** |ログ収集方法がファイルの場合|
| `paths` |ログ収集先のパス（例：/var/log/sample/*.log） `/var/log/*.log` |
| `exclude_paths` |ログ収集除外ターゲットパス|
| **kubernetes** |ログ収集方法がkubernetesの場合|
| `namespace` |ログ収集先の名前空間| `*` |
| `pod` |ログ収集先のパード名| `*` |
| `container` |ログ収集先のコンテナ名`*` |

キューバネティス環境では、ログエージェントは2つの方法でログを収集できます。

* **kubernetes** : ログを収集するワークロードの情報からログを収集します。収集するワークロードの名前空間、パード名、コンテナ名を設定します。 （ログエージェントをDaemonsetとして配布する場合は、その方法を使用してください。）
***file**: ログファイルを直接収集します。収集するログをログエージェントに直接マウントし、収集ログファイルの相対パスを設定します。 （ログエージェントをSidecarパターンとして展開する場合は、その方法を使用してください。）

たとえば、Cubernetes ワークロード情報からログを収集する場合は、次のように構成ファイルを作成できます。


```shell
cat << EOF >> ~/datasaker/config.yaml
logAgent:
  enabled: true
  logs:
    - collect:
        type: kubernetes
        kubernetes:
          - namespace: default
            pod: awesome-saker-5f4b7f7b4f-2q9qz
            container: awesome-saker
EOF
```
ログファイルを直接収集する場合は、次のように構成ファイルを作成できます。
```shell
cat << EOF >> ~/datasaker/config.yaml
logAgent:
  enabled: true
  logs:
    - collect:
        type: file
        file:
          paths:
           - /var/log/awesome_saker/*.log
EOF
```
### 2. Log agentのインストール
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker -f ~/datasaker/config.yaml
```
