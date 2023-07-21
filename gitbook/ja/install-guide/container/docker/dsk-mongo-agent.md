# dsk-mongo-agent

## Docker環境にDataSaker Mongo agentをインストールする

`Mongo agent`をドッカー環境にインストールする方法について説明します。

## DataSaker 先行作業を行いましたか？

現在Docker環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](dsk-mongo-agent/ja/$%7BPREPARATION\_MANUAL\_JP%7D/)

## Mongo Agentのインストール

### 1. Mongo agent 設定値登録

エージェントを接続するには、収集したいモンゴルDBサーバーのホスト、ポートアドレスをAgentに設定する必要があります。

```shell
 DSK_MONGO_URI=mongodb://<user>:<password>@<host>:<port>
```

たとえば、アドレスが `192.168.123.132` で、 `27017` ポートにサービス中のモンゴル DB を収集するためには次のように設定できます。

```shell
 DSK_MONGO_URI=mongodb://192.168.123.132:27017
```

端末に次のコマンドを入力して、dsk-mongo-agent 設定ファイルを生成します。

```shell
cd~
mkdir .datasaker
DSK_MONGO_URI=mongodb://localhost:27017
cat << EOF> ~/.datasaker/mongo-config.yml
agent:
  metadata:
    agent_name: dsk-mongo-agent
  option:
    exporter_config:
      command: "/etc/datasaker/target-exporter"
      args:
        - --collect-all
        - --mongodb.uri=${DSK_MONGO_URI}
    scrape_configs:
      - job_name: dsk-mongo-agent
        url: localhost:19216
        filtering_configs:
          rule: drop
EOF
```

| Argument |説明|
| ------------- | ------------------------ |
| --collect-all |あらゆる種類のメトリックを収集するように設定します。 |
| --mongodb.uri | mongodbのアドレスを設定します。 |

### 2. Mongo agentのインストール

1. データセーカが使用するローカルディレクトリを作成します。

    ```shell
     sudo mkdir -p /var/datasaker
     sudo chown -R datasaker:datasaker /var/datasaker/
    ```
2. ドッカー命令をサーバに入力します。

    ```shell
     docker run -d --name dsk-mongo-agent\
     -v /var/datasaker/:/var/datasaker/\
     -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
     -v ~/.datasaker/mongo-config.yml:/etc/datasaker/agent-config.yml:ro\
     -e DSK_LOG_LEVEL=Info\
     --restart=always\
     datasaker/dsk-mongo-agent
    ```