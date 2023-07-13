# dsk-log-agent

`Log agent`は、さまざまな環境でシステムまたはアプリケーションによって生成されたログデータをほぼリアルタイムで収集、処理、転送するエージェントです。 `Log agent`を介して複数のサーバーから生成されたログデータを集中的に管理および分析するために使用できます。これにより、ユーザーはシステムまたはアプリケーションで発生する問題をすばやく検出して対応できます。さらに、ログデータを分析することで、セキュリティ、パフォーマンス、ビジネス分析など、さまざまな目的に活用できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在のUbuntu環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker 先行操作]($%7BPREPARATION\_MANUAL\_KR%7D/)

## Log agentのインストール

`Log agent`は現在、Ubuntu 18.04.6 LTS(Bionic Beaver)環境でのみサポートしています.

### 1. パッケージのインストール

`DataSaker`の `Log agent`をインストールするにはsudo権限が必要です。

```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/dsk-log-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-log-agent
```

### 2. agent-configの設定

`/etc/datasaker/dsk-log-agent/agent-config.yml` パスに agent-config ファイルを次のように作成します。

`` yaml
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
      tag: 'sample'
      service:
        name: 'sample service'
        category: 'database'
        type: 'postgres'
        address: '0.0.0.0:5432'
```

#### **\[注意]**

1.必ず1つ以上のログ収集パス（ `path`）を入力してください。ログ収集パスを作成しないと、 `Log agent`が正常に動作しない可能性があります。
2. 次のように特定のパスのすべてのログを収集するように設定すると、ログエージェントに負荷がかかります。 （`/var/log/*`）収集ログファイルを個別に作成することをお勧めします（ `/var/log/containers/sampleApp.log`）
3. ログファイル以外のファイルがパスに設定されないように設定します。ログ収集パスのファイルを指定するか、それ以外のファイルを除外します。ログ収集設定にフォーマットを必ず作成してください。 （例： `.log`）

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

### 3. パッケージの実行

```shell
sudo systemctl enable dsk-log-agent --now
```

>注意事項\
>
>
> *ログ収集のためにパッケージを実行する前にエージェント設定ファイルを設定する必要があります（ `global-config.yml`、 `config.conf`）。
>
> *ログエージェントはデフォルトで `fluentd`を使ってログを収集します。すでに使用されている `fluentd` がある場合は、`DSK_FLUENTD_CMD` 環境変数を設定して変更できます。環境変数を使用して実行する場合は、次のように実行します。 (`sudo-E dsk-log-agent start`)

### 4. パッケージ実行状態の確認

```shell
systemctl status dsk-log-agent
```

または

```shell
service dsk-log-agent status
```

***

## Log agentを削除する

### 1. パッケージの中断

```shell
systemctl stop dsk-log-agent
```

または

```shell
service dsk-log-agent stop
```

### 2. パッケージの削除

パッケージを削除する前に、必ずパッケージを中断して削除してください。

```shell
sudo apt remove dsk-log-agent
```