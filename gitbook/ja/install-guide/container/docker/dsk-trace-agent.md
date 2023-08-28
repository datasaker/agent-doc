# dsk-trace-agent

## Docker環境にDatasaker Trace agentをインストールする

`Trace agent`は、`Opentelemetry`や`Jaeger`などのオープンソースの分散追跡システムと連携して、アプリケーションの分散追跡データを収集します。これにより、アプリケーション内のさまざまなサービス間の通信を追跡し、パフォーマンスのボトルネックを特定して最適化できます。収集されたデータは迅速に処理され、リアルタイムで監視および分析が可能です。お客様のニーズに合わせて`トレースエージェント`設定を調整して、最適な結果を提供します。

## DataSaker先行作業を進めましたか？

現在、Docker環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](README.md)

## Trace agentのインストール

Trace agent は内部的に次のポートを使用しています。

|ポート| Protocol | Describe |
| ----- | -------- | -------------- |
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

上記で必要なポートを公開してTrace agentを展開できます。\
たとえば、otlpデータのみを受け取ると、次のように配布できます。
```shell
     docker run -d --name dsk-trace-agent\
       -v /var/datasaker/:/var/datasaker/\
       -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
       -e DKS_LOG_LEVEL=info\
       -p 4317:4317/tcp\
       -p 4318:4318/tcp\
       --restart=always\
       datasaker/dsk-trace-agent
```
## Trace agentの設定

必要に応じて、以下のドキュメントを参照してトレースエージェントの設定を変更できます。

### Trace agent設定値

Trace agentの設定値の意味するデフォルト値は次のとおりです。ユーザーごとにエージェント設定に異なる要件があります。したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。最適な結果を得るためにエージェント設定を調整したら、エージェントの展開時にボリュームに設定ファイルをマウントして使用します。エージェント設定ファイルのパスは `/etc/datasaker/dsk-trace-agent/agent-config.yml` です。設定ファイルに値を追加または変更します。

ファイルの構造は以下の通りです。

#### `agent-config.yml`
```yaml
agent:
  metadata: <metadata>
  option:
    [ collector_config: <collector_config> ]
  [ reciever_config: <reciever_config> ]
```
**`metadata`**
```yaml
[ agent_name: <string> | default = "dsk-trace-agent" ]

[ cluster_id: <cluster_id> | default = "unknown" ]
```
**`collector_config`**
```yaml
[ sampling_rate: <float> | default = 1 ]
```
**`receiver_config`**
```yaml
[ listen_port: <uint16> | default = 14251 ]

[ custom_tags: <map[string]string> | default = "" ]
```
## ApplicationにTrace Agentを連携する

連携のためには、ターゲットアプリケーションに次の環境変数設定が必要です。

|環境変数値|説明
| -------------------------------------- | ---------------------------------------------- | ------------------ |
| OTEL\_EXPORTER\_OTLP\_TRACES\_ENDPOINT | （トレースエージェント）：（ポート）| trace-agentアドレス|
| OTEL \ _SERVICE \ _NAME | your-service-name |画面に表示したいサービス名|
| OTEL \ _METRICS \ _EXPORTER |なし不要メトリックデータの生成を防ぐため
| OTEL \ _LOGS \ _EXPORTER |なし不要なログデータの生成を防ぐため
| OTEL\_RESOURCE\_ATTRIBUTES | dsk\_host\_key=$(cat /var/datasaker/host\_key) | - |

たとえば、次のようにアプリケーションをデプロイできます。
```shell
docker run my-java-application \
    -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=<trace-agent-address>:4317 \
    -e OTEL_SERVICE_NAME=your-service-name \
    -e OTEL_METRICS_EXPORTER=none \
    -e OTEL_LOGS_EXPORTER=none \
    -e OTEL_RESOURCE_ATTRIBUTES=dsk_host_key=$(cat /var/datasaker/host_key) \
    -d
```
詳細については、以下を参照してください。

[関連ドキュメントリンク]（https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation）