# Coming soon

<!--
# Install DataSaker PostgreSQL Agent on Docker

This topic describes how to install **DataSaker PostgreSQL Agent** on Docker.

1. Create a local directory for **DataSaker** to use

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. Set the host and port address of the PostgreSQL server you want to collect.

    ```shell
     export DSK_PG_URI=postgrseql://<user>:<password>@<host>:<port>/postgres?sslmode=[disable|require|verify-ca|verify-full]
    ```

    For example, if you want to collect an PostgreSQL server that is running on port 5432 on localhost, set the following.

    ```shell
     export DSK_PG_URI=postgrseql://localhost:5432/postgres?sslmode=disable
    ```

3. Enter the following docker command to server.

   ```shell
    docker run -d --name saker-postgres-agent\
     -v /var/datasaker/:/var/datasaker/\
     -e DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}\
     -e DSK_GLOBAL_GATEWAY_URL=${VAR_GLOBAL_GATEWAY_URL}\
     -e DSK_GLOBAL_AGENTMANAGER_URL=${VAR_GLOBAL_AGENTMANAGER_URL}\
     -e DATA_SOURCE_NAME=${DSK_PG_URI}\
     --network=host\
     --restart=always\
     nexus2.exem-oss.org/saas/dsk-postgres-agent
   ```
-->