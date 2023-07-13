# Coming soon

<!--
# Install Datasaker Elasticsearch Agent on Docker

This topic describes how to install **Datasaker Elasticsearch Agent** on Docker.

1. Create a local directory for Datasaker to use

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. Set the host and port address of the Elasticsearch server you want to collect.

   ```shell
    export DSK_ES_URI=<protocol>://<user>:<password>@<host>:<port>
   ```

    For example, if you want to collect an Elasticsearch server that is running on port 9200 on localhost, set the following.

   ```shell
    export DSK_ES_URI=http://localhost:9200
   ```

3. Enter Docker command on server

   ```shell
    docker run -d --name saker-elasticsearch-agent\
    -v /var/datasaker/:/var/datasaker/\
    -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
    -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
    -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
    --restart=always\
    nexus2.exem-oss.org/saas/dsk-elasticsearch-agent\
    --es.uri=${DSK_ES_URI}\
    --es.all\
    --es.indices\
    --es.indices_settings\
    --es.indices_mappings\
    --es.cluster_settings\
    --es.shards\
    --es.snapshots
   ```
-->