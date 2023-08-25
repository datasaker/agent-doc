# dsk-elasticsearch-agent

## Kubernetes環境でDataSaker Elasticsearch agentをインストールする

`Elasticsearch agent`は、DataSakerからelasitcsearch情報を収集するエージェントです。

## DataSaker 先行作業を行いましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](README.md)

## Elasticsearch agentのインストール

### 1. Elasticsearch agent 設定値の登録

elasticsearchAgent.list\[].uri に elasticsearch addresss 情報を必ず登録してください
```shell
cat << EOF >> ~/datasaker/config.yaml

elasticsearchAgent:
  list:
    - name: 'es-1'
      uri: 'https://es_id:es_pw@elasticsearch_service.namespace.svc.cluster.local:9200'
      tolerations: []
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19114
      exporterArgs:
        - --es.timeout=5s
      extraArgs: []
      extraEnvs:
        - name: ENV_1
          value: ENV_1
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
```
各設定値の詳細な説明は以下の通りである。

|設定|説明
| ------------ | ----------------- |
|名前|エージェントの名前
| uri | elasticsearchアドレス|
| tolerations |キューバネティスのトロール設定|
| imgPolicy |キューバネティス画像ポリシー設定|
| imgVersion |エージェントイメージのバージョン設定|
| logLevel |エージェントログレベル設定|
| listenPort |エージェントポート設定|
| exporterArgs |エージェントオプション設定|
| extraArgs |エージェント追加オプションの設定|
| extraEnvs |エージェント追加環境変数の設定|
|リソース|エージェントクーバーネティスリソース設定|

### 2. Elasticsearch agentのインストール
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
詳細情報が必要な場合は、次の記事を参照してください。 [関連文書](../../../settings/dsk-elasticsearch-agent/settings.md)