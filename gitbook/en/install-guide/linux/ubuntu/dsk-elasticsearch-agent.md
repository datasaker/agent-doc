# Installing DataSaker Elasticsearch agent in Ubuntu environment (Beta)
`Elasticsearch agent` collects elasticsearch state information.

# Did you run the DataSaker predecessor?
In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](${PREPARATION_MANUAL_KR})

# Install the Elasticsearch agent
## 1. Install the package
sudo permission is required to install `Elasticsearch agent` of `DataSaker`.
<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-elasticsearch-agent
```

## 2. Register Elasticsearch agent settings

```shell
vi /etc/datasaker/dsk-elasticsearch-agent/agent-config.yml
```

Modify the following as needed.

```yaml
# Elasticsearch agent configuration file
agent:
  agent_name: "dsk-elasticsearch-agent" # agent name (alias) default=dsk-elasticsearch-agent
```

## 3. Run the package

```bash
systemctl start dsk-elasticsearch-agent
```

## 4. Check Package Execution Status

```bash
systemctl status dsk-elasticsearch-agent
```

or

```bash
service dsk-elasticsearch-agent status
```

# Arguments

You can modify it according to your environment by changing the argument settings of the elasticsearch exporter. The setting values ​​are as follows.

| Arguments | Description | Default |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| es.uri | Address (host and port) of the Elasticsearch node we should connect to. This could be a local node (localhost:9200, for instance), or the address of a remote Elasticsearch server. When basic auth is needed, specify as: <proto>://<user>:<password>@<host>:<port>. E.G., http://admin:pass@localhost:9200. Special characters in the user credentials need to be URL-encoded. | http://localhost:9200 |
| es.all | If true, query stats for all nodes in the cluster, rather than just the node we connect to. | false |
| es.cluster_settings | If true, query stats for cluster settings. | false |
| es. indices | If true, query stats for all indices in the cluster. | false |
| es.indices_settings | If true, query settings stats for all indices in the cluster. | false |
| es.indices_mappings | If true, query stats for mappings of all indices of the cluster.| false |
| es. aliases | If true, include informational aliases metrics. | true |
| es.shards | If true, query stats for all indices in the cluster, including shard-level stats (implies es.indices=true). | false |
| es.snapshots | If true, query stats for the cluster snapshots. | false |
| es.slm | If true, query stats for SLM. | false |
| es.data_stream | If true, query state for Data Steams. | false |
| es. timeout | Timeout for trying to get stats from Elasticsearch. (ex: 20s) | 5s |
| es.ca | Path to PEM file that contains trusted Certificate Authorities for the Elasticsearch connection. | |
| es.client-private-key | Path to PEM file that contains the private key for client auth when connecting to Elasticsearch. | |
| es.client-cert | Path to PEM file that contains the corresponding cert for the private key to connect to Elasticsearch. | |
| es.clusterinfo.interval | Cluster info update interval for the cluster label | 5 m |
| es.ssl-skip-verify | Skip SSL verification when connecting to Elasticsearch.| false |
| aws.region | Region for AWS elasticsearch | |

---
# Remove the Elasticsearch agent
## 1. Package abort
```bash
systemctl stop dsk-elasticsearch-agent
```
or
```bash
service dsk-elasticsearch-agent stop
```

## 2. Remove packages
```bash
sudo apt remove dsk-elasticsearch-agent
```