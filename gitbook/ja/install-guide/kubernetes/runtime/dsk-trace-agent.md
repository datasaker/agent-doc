# dsk-trace-agent

## Kubernetes環境でDataSaker Trace agentをインストールする

`Trace agent`は、`Opentelemetry`や`Jaeger`などのオープンソースの分散追跡システムと連携して、アプリケーションの分散追跡データを収集します。これにより、アプリケーション内のさまざまなサービス間の通信を追跡し、パフォーマンスのボトルネックを特定して最適化できます。収集されたデータは迅速に処理され、リアルタイムで監視および分析が可能です。お客様のニーズに合わせて`トレースエージェント`設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](README.md)

## Trace agentのインストール

### 1. Trace agent 設定値の登録
```shell
cat << EOF >> ~/datasaker/config.yaml

traceAgent:
  enabled: true
  logLevel: 'INFO'
EOF
```
### 2. Trace agentのインストール
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
## Trace agent ポート情報

|ポート| Protocol | Describe |
| ----- | -------- | -------------- |
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

## Trace agentの設定

### Trace agent設定値

Trace agentの設定値の意味とdefault値は次のとおりです。ユーザーごとにエージェント設定に異なる要件があります。したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。最適な結果を得るためにエージェント設定を調整してください。各項目に設定される値はデフォルト設定値です。
```yaml
traceAgent:
  enabled: false
  tolerations: []
  imgPolicy: 'Always' # [Always, IfNotPresent, Never]
  imgVersion: 'latest'
  tags: []
  resource:
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1000m
      memory: 1000Mi
  logLevel: 'INFO'
  nodeSelector: {}
  affinity: {}
  collector:
    samplingRate: 1
```
各設定値の詳細な説明は以下の通りである。

|設定値|説明
| ----------- | ----------------------------------------------------------------------- |
| enabled | Trace agentの有効化を設定します。 |
| tolerations |ワーカーノードにtaintが設定されている場合は、toleration設定を追加します。 |
| imgPolicy |エージェントのイメージポリシーを設定します。 （Always、IfNotPresent、Never）|
| imgVersion |エージェントのImage Versionを設定します。 |
| logLevel | agentのlog levelを設定します。 （debug> info> warn> error> panic> fatal）|
|タグ|ユーザー設定でデータに追加するタグのリストを入力します。 key1 = value1、key2 = value2 |
|リソース| agentのリソースを設定します。小さすぎると正常動作できないことがあります。 |

## ApplicationにTrace Agentを連携する

[関連ドキュメントリンク]（https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation）