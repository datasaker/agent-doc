# dsk-mongo-agent

## Install DataSaker Mongo agent in Docker environment

Describes how to install `Mongo agent` in Docker environment.

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Docker environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](dsk-mongo-agent/en/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Install Mongo Agent

### 1. Register Mongo agent settings

To connect the agent, the host and port address of the MongoDB server to be collected must be set in the agent.

```shell
 DSK_MONGO_URI=mongodb://<user>:<password>@<host>:<port>
```

For example, to collect MongoDB with address `192.168.123.132` and port `27017`, you can set as follows.

```shell
 DSK_MONGO_URI=mongodb://192.168.123.132:27017
```

Enter the following command in Terminal to create the dsk-mongo-agent configuration file.

```shell
cd ~
mkdir .datasaker
DSK_MONGO_URI=mongodb://localhost:27017
cat << EOF > ~/.datasaker/mongo-config.yml
agent:
  metadata:
    agent_name: dsk-mongo-agent
  options:
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

| Arguments | Description |
| ------------- | ------------------------ |
| --collect-all | Set up to collect all kinds of metrics. |
| --mongodb.uri | Set the address of mongodb. |

### 2. Install Mongo agent

1. Create a local directory for datasaker to use.

    ```shell
     sudo mkdir -p /var/datasaker
     sudo chown -R datasaker:datasaker /var/datasaker/
    ```
2. Enter the docker command into the server.

    ```shell
     docker run -d --name dsk-mongo-agent\
     -v /var/datasaker/:/var/datasaker/\
     -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
     -v ~/.datasaker/mongo-config.yml:/etc/datasaker/agent-config.yml:ro\
     -e DSK_LOG_LEVEL=Info\
     --restart=always\
     datasaker/dsk-mongo-agent
    ```