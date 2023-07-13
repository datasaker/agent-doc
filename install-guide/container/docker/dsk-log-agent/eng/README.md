# Coming soon

<!--
# Install Datasaker Log Agent on Docker

This topic describes how to install **Datasaker Log Agent** on Docker.

1. Create a local directory for Datasaker to use

   ```shell
    mkdir -p /var/datasaker
    chmod 777 /var/datasaker/ 
   ```

2. Mount the volume to the log agent container for the logs you want to collect.

3. Enter Docker command on server

   ```shell
   docker run -d --name saker-log-agent \
    -v /var/datasaker/:/var/datasaker/ \
    -v /var/lib/docker/containers/:/etc/datasaker/dsk-log-agent/log/containers/ \
    -e DSK_GLOBAL_APIKEY=${APIKEY}\
    -e DSK_GLOBAL_GATEWAY_URL=${DSK_GLOBAL_GATEWAY_URL}\
    -e DSK_GLOBAL_AGENTMANAGER_URL=${DSK_GLOBAL_AGENTMANAGER_URL}\
    --restart=always\
    nexus2.exem-oss.org/saas/dsk-log-agent \
    --global.config=/etc/datasaker/global-config.yml \
    --agent.config=/etc/datasaker/dsk-log-agent/agent-config.yml \
    --transport.tls.ca=/etc/datasaker/dsk-log-agent/cert/ca-cert.pem \
    --transport.tls.cert=/etc/datasaker/dsk-log-agent/cert/client-cert.pem \
    --transport.tls.key=/etc/datasaker/dsk-log-agent/cert/client-key.pem
   ```
-->