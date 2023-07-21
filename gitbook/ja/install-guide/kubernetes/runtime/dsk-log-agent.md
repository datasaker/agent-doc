# dsk-log-agent

## Kubernetes環境でDataSaker Log agentをインストールする

`Log agent`は、さまざまな環境でシステムまたはアプリケーションによって生成されたログデータをほぼリアルタイムで収集、処理、転送するエージェントです。 `Log agent`を介して複数のサーバーから生成されたログデータを集中的に管理および分析するために使用できます。これにより、ユーザーはシステムまたはアプリケーションで発生する問題をすばやく検出して対応できます。さらに、ログデータを分析することで、セキュリティ、パフォーマンス、ビジネス分析など、さまざまな目的に活用できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (dsk-log-agent/ja/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Log agentのインストール

デフォルトでは、 `Log agent` はキューバネティス環境でデーモンセットとして配布されます。したがって、すべてのノードに `Log agent` がインストールされます。特定のノードにのみLog agentをインストールしたい場合は、そのノードにaffinityまたはnodeSelectorを追加設定してください。

### 1. Log agent 設定値の登録

`Log agent`が正常に動作するためには、必ず****collect.workloads**_に** 必ず**1つ以上のログを収集するワークロードを設定する必要があります。

`Log agent`の設定値の意味とデフォルト設定値は次のとおりです。ユーザーごとにエージェント設定に異なる要件があります。したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。最適な結果を得るためにエージェント設定を調整してください。 `\~/datasaker/config.yaml`でその値を追加または変更します。

| **Settings** | **Description** | **Default** | **Required** |
| ----------------------------------- | --------------------------------------------------------------------------------------- | :---------: | :----------: |
| `logAgent.collect.workloads[]` |ログ収集対象のワークロード名（例： 'nginx'）| N / A | **✓** |
| `logAgent.collect.keywords` |ログ収集キーワード（キーワードを含むログのみを収集）| N / A | |
| `logAgent.collect.tag` |カスタムタグ| N / A | |
| `logAgent.collect.service.name` |サービス名| `default` | |
| `logAgent.collect.service.category` |サービス分類（ `app`、`database`、`syslog`、`etc`のいずれかの値を作成してください。）| `etc` | |
| `logAgent.collect.service.type` |サービスデータベースの種類と開発言語の種類（ `postgres`、`mysql`、`java`、`etc`のいずれかの値を書いてください） `etc` | |
| `logAgent.collect.service.address` |データベースのホストとポート情報（サービスの分類がデータベースの場合は設定します。設定しないと、特定の機能を使用できない可能性があります。） N / A | ⚠️|

```shell
cat << EOF >> ~/datasaker/config.yaml

logAgent:
  enabled: true
  logLevel: 'INFO'
  collect:
    - workloads:
      - WORKLOAD_NAME
      service:
        名前：SERVICE_NAME
        category: APP
        type: ETC
EOF
```

### 2. Log agentのインストール

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker -f ~/datasaker/config.yaml
```

## Log agentの使い方

### 1. 必ず、1 つ以上のログ収集対象ワークロード名 (`workloads`) を入力してください。

ワークロードを作成しないと、 `Log agent`が正常に動作しない可能性があります。

``` yaml
collect:
  - workloads: # [必須] 収集するワークロードの名前を入力します。
      - postgres
```

#### \[ **Workloads** 作成ガイド ]

ワークロードとは、クバネティスで駆動されるアプリケーションを意味します。 （ワークロードが単一のコンポーネントであるか、または連携して動作する複数のコンポーネントであるかにかかわらず、Cubernetesはワークロードを一連のPodセット内で実行します。）リソースを提供します。 (Deployment, Replicaset, StatefulSet, DaemonSet)

`collect.workloads`には、収集したいワークロードの名前を作成すると、対応するログファイルが収集されます。 (/var/log/containers/_WORKLOAD\_NAME_.log)

たとえば、`app-server`というワークロード名でPodがデプロイされた場合、そのPodのContainerログは、Workload名と一緒にハッシュ値が加算されたファイル名で作成されます。 （app-server-5f4b7f7b4f-2q9qz.log）対応するログを収集するには、`collect.workloads`に 'app-server'を作成すると自動的にそのログを収集します。

### 2. キーワード (`keywords`) 設定に注意してください。

`keywords`設定に登録した文字列を含むログのみを収集します。 1つ以上の `keywords`を設定した場合、1つの `keywords`が含まれている場合はログを収集します。

``` yaml
keywords: [] # 指定されたキーワードが、返されたログのみを収集するように設定します。
```

### 3. ログ収集対象の `type` が `database` の場合は `address` 設定をお勧めします。

この設定では、関連する `database agent` からログ情報をマッピングして表示します。 `address`を設定しないと、その機能を使用できない可能性があります。

``` yaml
service:
  name: custom-service-name
  category: データベース
  type: postgres
  address: 0.0.0.0:5432
```