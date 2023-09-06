# dsk-elasticsearch-agent

`elasticsearch-agent` は elasticsearch 状態情報を収集します。

## DataSaker 先行作業を行いましたか？

現在の環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker先行操作]（$ {PREPARATION_MANUAL_JP}）

## Elasticsearch agentのインストール

### 1. パッケージのインストール

`DataSaker`の `Elasticsearch agent`をインストールするにはsudo権限が必要です。
<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-elasticsearch-agent
```
### 2. Elasticsearch agent 設定値の登録
```shell
vi /etc/datasaker/dsk-elasticsearch-agent/agent-config.yml
```
必要に応じて次の内容を修正します。
```yaml
agent:
  agent_name: "dsk-base-agent"
  cluster_id: "my-cluster-id"
```
各設定の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent_name` |エージェント名（エイリアス）| dsk-base-agent |いいえ
| `cluster_id` |管理対象となる環境がどのクラスタにまとめられているかを設定します。 | unknown |いいえ

### 3. パッケージの実行
```bash
systemctl enable dsk-elasticsearch-agent --now
```
### 4. パッケージ実行状態の確認
```bash
systemctl status dsk-elasticsearch-agent
```
または
```bash
service dsk-elasticsearch-agent status
```
## Arguments

elasticsearch exporter の argument 設定を変更して、ご使用の環境に合わせて変更できます。設定値は以下の通りです。

| Argument |説明|デフォルト|
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------ |
| es.uri | Address (host and port) of the Elasticsearch node we should connect to. This could be a local node (localhost:9200, for instance), or the address of a remote Elasticsearch server. When basic auth is needed, specify as: <proto>://<user>:<password>@<host>:<port>. E.G.、http://admin:pass@localhost:9200。 Special characters in the user credentials need to be URL-encoded. | http://localhost：9200 |
| es.all | If true, query stats for all nodes in the cluster, rather than just the node we connect to. | false |
| es.cluster_settings | If true, query stats for cluster settings. | false |
| es.indices | If true, query stats for all indices in the cluster. | false |
| es.indices_settings | If true, query settings stats for all indices in the cluster. | false |
| es.indices_mappings | If true, query stats for mappings of all indices of the cluster. | false |
| es.aliases | If true, include informational aliases metrics. | true || es.shards | If true, query stats for all indices in the cluster, including shard-level stats (implies es.indices=true). | false |
| es.snapshots | If true, query stats for the cluster snapshots. | false |
| es.slm | If true, query stats for SLM. | false |
| es.data_stream | If true, query state for Data Steams. | false |
| es.timeout | Timeout for trying to get stats from Elasticsearch. （例：20秒）| 5秒|
| es.ca | Path to PEM file that contains trusted Certificate Authorities for the Elasticsearch connection. | |
| es.client-private-key | Path to PEM file that contains the private key for client auth when connecting to Elasticsearch. | |
| es.client-cert | Path to PEM file that contains the corresponding cert for the private key to connect to Elasticsearch. | |
| es.clusterinfo.interval | Cluster info update interval for the cluster label | 5m || es.ssl-skip-verify | Skip SSL verification when connecting to Elasticsearch. | false |
| aws.region | Region for AWS elasticsearch | |

---

## Elasticsearch agentを削除する

### 1. パッケージの中断
```bash
systemctl stop dsk-elasticsearch-agent
```
または
```bash
service dsk-elasticsearch-agent stop
```
## 2. パッケージの削除
```bash
sudo apt remove dsk-elasticsearch-agent
```
