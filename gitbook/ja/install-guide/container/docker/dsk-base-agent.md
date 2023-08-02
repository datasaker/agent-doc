# dsk-base-agent

## Docker環境にDataSaker Base agentをインストールする

`Base agent`はサーバから発生する様々な情報をリアルタイムで収集します。たとえば、メモリ、CPU使用率など、サーバーのパフォーマンス指標、ネットワークトラフィック、コンテナ情報など、さまざまな情報を収集できます。これにより、お客様はリアルタイムでサーバーの状態を監視でき、サーバーのパフォーマンスを最適化し、信頼性を向上させることができます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在、Docker環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (dsk-base-agent/ja/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Base agentのインストール

Cluster IDを設定すると、リソースを論理グループとして管理できます。 Cluster IDが設定されていない場合は、デフォルトとして `unknown`が設定されます。

Cluster IDを設定するには、次のように環境変数を設定する必要があります。
```shell
export DSK_CLUSTER_ID=my-cluster
```
1. データセーカが使用するローカルディレクトリを作成します。
```shell
   sudo mkdir -p /var/datasaker
   sudo chown -R datasaker:datasaker /var/datasaker/ 
```
2. ドッカー命令をサーバに入力します。
```shell
docker run -d --name dsk-node-agent\
   -v /var/datasaker/:/var/datasaker/\
   -v /proc/:/host/proc/:ro\
   -v /sys/:/host/sys/:ro\
   -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
   -e DSK_CLUSTER_ID=${DSK_CLUSTER_ID} \
   --privileged\
   --restart=always\
   --network host\
   --pid host\
   datasaker/dsk-node-agent
docker run -d --name dsk-container-agent\
   -v /var/datasaker/:/var/datasaker/\
   -v /:/rootfs/:ro\
   -v /var/run/:/var/run/:ro\
   -v /sys/:/sys/:ro\
   -v /dev/disk/:/dev/disk/:ro\
   -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
   -e DSK_CLUSTER_ID=${DSK_CLUSTER_ID} \
   -e GOMAXPROCS=1\
   --privileged\
   --restart=always\
   datasaker/dsk-container-agent

sudo chown -R datasaker:datasaker /var/datasaker/ 
```
