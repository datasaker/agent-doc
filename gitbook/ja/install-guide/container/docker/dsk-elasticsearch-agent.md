# dsk-elasticsearch-agent

## Docker環境にDataSaker Elasticsearch agentをインストールする

`Elasticsearch agent`は、DataSakerからelasitcsearch情報を収集するエージェントです。

## DataSaker先行作業を進めましたか？

現在、Docker環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](README.md)

## Elasticsearch agentのインストール

### 1. Elasticsearch agent 設定値の登録

エージェントを接続するには、収集したいElasticsearchサーバーのホストとポートアドレスをエージェントに設定する必要があります。
```shell
 DSK_ES_URI=http(s)://<user>:<password>@<host>:<port>
```
たとえば、localhost 9200ポートにサービスしているElasticsearchを収集するには、次のように設定できます。
```shell
 DSK_ES_URI=http://localhost:9200
```
端末に次のコマンドを入力して、dsk-elasticsearch-agent設定ファイルを生成します。
```shell
cd ~
mkdir .datasaker
DSK_ES_URI=http://localhost:9200
cat << EOF > ~/.datasaker/elasticsearch-config.yml
agent:
  metadata:
  # agent_name: dsk-elasticsearch-agent
  option:
    exporter_config:
      command: "/etc/datasaker/target-exporter"
      args:
        - --es.uri=${DSK_ES_URI}
        - --es.all
        - --es.cluster_settings
        - --es.indices_settings
        - --es.indices_mappings
        - --es.shards
        - --es.snapshots
    scrape_configs:
      - job_name: dsk-elasticsearch-agent
        url: localhost:19114
        filtering_configs:
          rule: drop
EOF
```
上記の設定では、各 argument は次のことを意味します。

| Argument |説明|デフォルト|
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| es.uri |接続する必要があるElasticsearchノードのアドレス（ホストとポート）。このアドレスはローカルノード（localhost：9200など）、リモートElasticsearchサーバーのアドレスです。基本認証が必要な場合は、次のように指定します。://<ユーザー>：<パスワード> @ <ホスト>：<ポート>。例: http://admin:pass@localhost:9200.ユーザー資格情報の特殊文字はURLエンコードする必要があります。 | http://localhost：9200 |
| es.all | true の場合、接続したノードだけを照会するのではなく、クラスター内のすべてのノードの統計を照会します。 | false |
| es.cluster\_settings | true の場合は、クラスター設定に関する統計を照会します。 | false |
| es.indices\_settings | true の場合、クラスタ内のすべてのインデックスの設定統計を照会します。 | false |
| es.indices \ _mappings | true の場合、クラスター内のすべての索引マッピングに関する統計を照会します。 | false |
| es.shards | trueの場合、シャードレベルの統計を含むクラスタ内のすべてのインデックスの統計を照会します。 | false |
| es.snapshots | trueの場合、クラスタスナップショットの統計情報を照会します。 | false |

### 2. Elasticsearch agentのインストール

1. データセーカが使用するローカルディレクトリを作成します。

    ```shell
     sudo mkdir -p /var/datasaker
     sudo chown -R datasaker:datasaker /var/datasaker/
    ```

2. ドッカー命令をサーバーに入力します。

    ```shell
    docker run -d --name dsk-elasticsearch-agent\
       -v /var/datasaker/:/var/datasaker/\
       -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
       -v ~/.datasaker/elasticsearch-config.yml:/etc/datasaker/dsk-elasticsearch-agent/agent-config.yml:ro\
       -e DSK_LOG_LEVEL=INFO\
       -e DSK_SUB_KIND=elasticsearch-1\
       --restart=always\
       datasaker/dsk-elasticsearch-agent
    ```