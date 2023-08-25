# dsk-trace-agent

`trace-agent`はopentelemetryやJaegerのようなオープンソースの分散追跡システムと連携して、アプリケーションの分散追跡データを収集します。
これにより、アプリケーション内のさまざまなサービス間の通信を追跡し、パフォーマンスのボトルネックを特定して最適化できます。
収集されたデータは迅速に処理され、リアルタイムで監視および分析が可能です。
お客様のニーズに合わせて`Trace Agent`設定を調整して、最適な結果を提供します。

## Datasaker先行作業を行いましたか？

現在の環境で `DataSaker` の先行操作が進まない場合は、`DataSaker` 先行操作を先に進めてください。 [DataSaker先行操作](README.md)

## Trace agentのインストール

### 1. パッケージのインストール
```shell
yum install dsk-trace-agent
```
### 2. Trace agentの設定
```shell
vi /etc/datasaker/dsk-trace-agent/agent-config.yml
```
必要に応じて次の内容を修正します。
```yaml
agent:
  agent_name: "dsk-trace-agent"
```
### 3. パッケージの実行
```shell
systemctl enable dsk-trace-agent --now
```
### 4. パッケージ実行状態の確認
```shell
systemctl status dsk-trace-agent
```
## Trace agent ポート情報

|ポート| Protocol | Describe |
|-------|----------|----------------|
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

## アプリケーションにトレースエージェントを連携する

Trace agentを利用するには、まずSDKでアプリケーションをインストルメントする必要があります。
詳しくはリンクをご覧ください。
[関連ドキュメントリンク]（https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation）

## Trace agentの設定

必要に応じて、以下のドキュメントを参照してトレースエージェントの設定を変更できます。

### Trace agent設定値

Trace agentの設定値の意味するデフォルト値は次のとおりです。
ユーザーごとにエージェント設定に異なる要件があります。
したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。
最適な結果を得るためにエージェント設定を調整してください。
エージェント設定ファイルのパスは `/etc/datasaker/dsk-trace-agent/agent-config.yml` です。
設定ファイルに値を追加または変更します。

ファイルの構造は以下の通りです。

### `agent-config.yml`
```yaml
agent:
  metadata: <metadata>
  option:
    [ collector_config: <collector_config> ]
  [ reciever_config: <reciever_config> ]
```
#### `metadata`
```yaml
[ agent_name: <string> | default = "dsk-trace-agent" ]

[ cluster_id: <cluster_id> | default = "unknown" ]
```
各設定値の詳細な説明は以下の通りである。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent_name` |エージェントの名前を設定します。 | dsk-trace-agent |いいえ
| `cluster_id` |管理対象となる環境がどのクラスタにまとめられているかを設定します。 | unknown |いいえ

#### `collector_config`
```yaml
[ sampling_rate: <float> | default = 1 ]
```
各設定の詳細な説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `sampling_rate` | collector に適用されるサンプリング率を設定します。 100以上の場合、すべてのデータが収集されます。 | 1 |いいえ

#### `receiver_config`
```yaml
[ custom_tags: <map[string]string> | default = "" ]
```
各設定の詳細な説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | ----------- | ------------ |
| `listen_port` | collectorからデータを受け取るポート番号を設定します。 | 14251 |いいえ
| `custom_tags` |各スパンに適用されるカスタムタグを設定します。 | "" |いいえ

### Example
```yaml
agent:
  metadata:
    agent_name: dsk-trace-agent 
  option:
    collector_configs:
    sampling_rate: 1
```
## Trace agentを削除する

### 1. パッケージの中断
```shell
systemctl stop dsk-trace-agent
```
### 2. パッケージの削除
```shell
yum remove dsk-trace-agent
```
