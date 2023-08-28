# dsk-log-agent

## Kubernetes環境でDataSaker Log agentをインストールする

`Log agent`は、さまざまな環境でシステムまたはアプリケーションによって生成されたログデータをほぼリアルタイムで収集、処理、転送するエージェントです。 `Log agent`を介して複数のサーバーから生成されたログデータを集中的に管理および分析するために使用できます。これにより、ユーザーはシステムまたはアプリケーションで発生する問題をすばやく検出して対応できます。さらに、ログデータを分析することで、セキュリティ、パフォーマンス、ビジネス分析など、さまざまな目的に活用できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker先行作業を進めましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](README.md)

## Log agentのインストール

デフォルトでは、 `Log agent`はクーバネティス環境でデフォルトでデーモンセットとして配布されます。したがって、すべてのノードに `Log agent` がインストールされます。特定のノードにのみ `Log agent` をインストールしたい場合は、そのノードに affinity または nodeSelector を追加設定してください。

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
| `tag` |ログ収集対象のタグ|
| `keyword` |ログ収集キーワード（キーワードを含むログのみを収集）| |
| **マルチライン** |マルチラインログ収集設定| |
| `format` |マルチラインログフォーマット（例：go、java、python）| |
| `pattern` |マルチラインログパターン（例：^\d{4}-\d{2}-\d{2}） - ユーザーカスタム正規表現パターンが利用可能です。 |
| **masking** |機密情報ログマスキング設定| |
| `pattern` |マスクするログパターン（たとえば、^\d{4}-\d{2}-\d{2}） - ユーザーカスタム正規表現パターンを使用可能|
| `replace` |マスキングパターンを置き換える文字列（例：*****） |
| **collect** |ログ収集先設定| |
| `type` |ログ収集方法（ `file`、`driver`、`kubernetes`のいずれかの値を作成する）| `file` |
| `カテゴリー`|サービス分類（ `app`、`database`、`syslog`、`etc`のいずれかの値を作成する） `etc` |
| `address` |データベースホストとポート情報（サービス分類がデータベースの場合は設定） |
| **ファイル** |ログ収集方法がファイルの場合|
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

次のマニフェストファイルで構成されるワークロードがある場合、

 ``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  レプリカ：3
  セレクター：
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
 ```

次のようにログエージェントYAML設定ファイルを作成できます。
```shell
cat << EOF >> ~/datasaker/config.yaml
logAgent:
  enabled: true
  logs:
    - collect:
        type: kubernetes
        kubernetes:
          - namespace: default
            pod: nginx-deployment
            container: nginx
EOF
```
特定の名前空間/ポッド/コンテナ名のワークロードに対してすべてのログファイルを収集したい場合は、共通のキーワードのみを作成して、そのキーワードを含むすべてのワークロードのログを収集できます。

すべての名前空間にnginxという名前を含むポッドのすべてのコンテナのログを収集したい場合は、次のように設定ファイルを作成できます。
(注意: namespace/pod/container に値を作成しない場合は、すべての namespace/pod/container のすべてのコンテナのログを収集します.)
```shell
cat << EOF >> ~/datasaker/config.yaml
logAgent:
  enabled: true
  logs:
    - collect:
        type: kubernetes
        kubernetes:
          - pod: nginx
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
           - /var/log/containers/nginx-deployment*default*nginx*.log
EOF
```
### 2. Log agentのインストール
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker -f ~/datasaker/config.yaml
```
## Log agent ログ収集の設定

`Log agent`は、エージェント構成YAMLファイルを介したマルチラインログ収集とログマスキング設定をサポートします。

### 1. マルチラインログ収集設定

ログエージェントの設定YAMLファイルのマルチラインログ収集設定は2つの方法でサポートされています。

***format**: マルチラインログのフォーマットを設定します。現在、`go`、`java`、`python`の3つのフォーマットをサポートしています。
***pattern**: マルチラインログのパターンを設定します。ユーザーカスタム正規表現パターンを使用できます。
```yaml
logs:
  - multiline:
      format:
      pattern: []
```
たとえば、次のようなマルチラインログがある場合、
```shell
Dec 14 06:41:08 Exception in thread "main" java.lang.RuntimeException: Something has gone wrong, aborting!
    at com.myproject.module.MyProject.badMethod(MyProject.java:22)
    at com.myproject.module.MyProject.oneMoreMethod(MyProject.java:18)
    at com.myproject.module.MyProject.anotherMethod(MyProject.java:14)
    at com.myproject.module.MyProject.someMethod(MyProject.java:10)
    at com.myproject.module.MyProject.main(MyProject.java:6)
```
`format`設定により、次のようにマルチラインログを収集できます。
```yaml
logs:
  - multiline:
      format: 'java'
```
**\[注意]** `format`を介してマルチラインログを収集する方法は、すべてのパターンのマルチラインログを収集することはできません。正確なマルチラインログ収集のためには、 `pattern`によるマルチラインログ収集の設定をお勧めします。

別の方法である `pattern` を介して複数行ログを収集する場合は、次のように書くことができます。
```yaml
logs:
  - multiline:
      pattern: 
        - '^\w*\s\d{1,2}\s\d{1,2}\:\d{1,2}\:\d{1,2}'
```
マルチラインログのログの先頭にパターンを作成すると、そのパターンで始まるログに基づいてマルチラインログが収集されます。

**\[注意]** ログエージェントは、複数行のログ収集で、2 行目から空白で始まると判断します。

### 2. 機密情報ログマスキングの設定

ログエージェントの設定YAMLファイルの機密情報ログマスキング設定をサポートします。

* **pattern**：マスクするログパターンを設定します。ユーザーカスタム正規表現パターンを使用できます。
* **replace**：マスキングパターンを置き換える文字列を設定します。
```yaml
logs:
  - masking:
      - pattern:
        replace:
```
たとえば、次のログがあり、
```shell
2023-08-18 06:35:38.993 GMT [739243] LOG:  statement:
            SELECT * 
            FROM address 
            WHERE id = '1234567890';
```
そのログで `statement:` の後に続く問合せステートメントを `PRIVATE_QUERY` 文字列でマスキングしたい場合は、 `pattern` に次のように正規表現パターンを作成してマスキングできます。
```yaml
logs:
  - masking:
      - pattern: 'statement: .*'
        replace: 'statement: PRIVATE_QUERY'
```
