# Coming soon

<!--
# Install Datasaker Mongo Agent on Docker

This topic describes how to install **Datasaker Mongo Agent** on Docker.

1. Create a local directory for Datasaker to use

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. Set the host and port address of the MongoDB server you want to collect.

   ```shell
    export DSK_MONGO_URI=mongodb://<user>:<password>@<host>:<port>
   ```

    For example, if you want to collect an MongoDB server that is running on port 27017 on localhost, set the following.

   ```shell
    export DSK_MONGO_URI=http://localhost:27017
   ```

3. Enter Docker command on server

   ```shell
    docker run -d --name saker-mongo-agent\
    -v /var/datasaker/:/var/datasaker/\
    -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
    -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
    -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
    --restart=always\
    nexus2.exem-oss.org/saas/dsk-mongo-agent\
    --mongodb.uri=${DSK_MONGO_URI}\
    --collect-all\
    --compatible-mode
   ```
-->