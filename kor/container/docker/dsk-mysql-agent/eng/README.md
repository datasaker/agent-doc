# Coming soon

<!--
# Install Datasaker MySQL Agent on Docker

This topic describes how to install **Datasaker MySQL Agent** on Docker.

1. Create a local directory for Datasaker to use

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. Set the host and port address of the MySQL server to be collected.

   ```shell
    export DSK_MYSQL_URI=<protocol>://<user>:<password>@<host>:<port>
   ```

   For example, to collect the MySQL server serviced on localhost 9200 port, set as follows.

   ```shell
    export DSK_MYSQL_URI=http://localhost:9200
   ```

3. Enter Docker command on server

   ```shell
    docker run -d --name saker-mysql-agent\
    -v /var/datasaker/:/var/datasaker/\
    -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
    -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
    -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
    --restart=always\
    nexus2.exem-oss.org/saas/dsk-mysql-agent\
    --es.uri=${DSK_MYSQL_URI}\
    --es.all\
    --es.indices\
    --es.indices_settings\
    --es.indices_mappings\
    --es.cluster_settings\
    --es.shards\
    --es.snapshots
   ```
-->