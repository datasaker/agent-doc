# dsk-elasticsearch-agent

## Install DataSaker Elasticsearch agent in Docker environment

`Elasticsearch agent` is an agent that collects elasticsearch information from DataSaker.

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Docker environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessors] (README.md)

## Install the Elasticsearch agent

### 1. Register Elasticsearch agent settings

In order to connect the agent, the host and port address of the Elasticsearch server to be collected must be set on the agent.
```shell
 DSK_ES_URI=http(s)://<user>:<password>@<host>:<port>
```
For example, to collect Elasticsearch serving on localhost 9200 port, you can set as follows.
```shell
 DSK_ES_URI=http://localhost:9200
```
Enter the following command in Terminal to create the dsk-elasticsearch-agent configuration file.
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
In the above setup, each argument means:

| Arguments | Description | Default |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| es.uri | The address (host and port) of the Elasticsearch node you need to connect to. This address can be either the local node (e.g. localhost:9200) or the address of a remote Elasticsearch server. If basic authentication is required, specify: ://<user>:<password>@<host>:<port>. Example: http://admin:pass@localhost:9200. Special characters in user credentials must be URL-encoded. | http://localhost:9200 |
| es.all | If true, query statistics for all nodes in the cluster, not just the nodes you connect to. | false |
| es.cluster\_settings | If true, queries statistics about cluster settings. | false |
| es.indices\_settings | If true, query setup statistics for all indexes in the cluster. | false |
| es.indices\_mappings | If true, queries statistics for all index mappings in the cluster. | false |
| es.shards | If true, queries statistics for all indexes in the cluster, including shard level statistics. | false |
| es.snapshots | If true, query statistics for cluster snapshots. | false |

### 2. Install the Elasticsearch agent

1. Create a local directory for datasaker to use.

    ```shell
     sudo mkdir -p /var/datasaker
     sudo chown -R datasaker:datasaker /var/datasaker/
    ```

2. Enter the docker command into the server.

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