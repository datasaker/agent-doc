# Configure DataSaker configuration file in Docker environment

## Register user information

Configure the environment for configuring `DataSaker` configuration files and register user tenant information.

First, configure the environment to configure the `DataSaker` configuration file.
Open a terminal on the server and enter the following command.
```shell
sudo groupadd -g 202306 datasaker
sudo useradd -r -u 202306 -g datasaker -s /usr/sbin/nologin datasaker

mkdir -p ~/.datasaker
chmod 755 ~/.datasaker
```
Then configure the configuration file using the API Key issued from `Datasaker`.
```shell
cat << EOF > ~/.datasaker/config.yml
global:
  api_key: "<YOUR_API_KEY>"
  gates:
    trace_datagate:
      url: gate.kr.datasaker.io:31300
    manifest_datagate:
      url: gate.kr.datasaker.io:31301
    metric_datagate:
      url: gate.kr.datasaker.io:31302
    plan_datagate:
      url: gate.kr.datasaker.io:31303
    loggate:
      url: gate.kr.datasaker.io:31304
  agent_manager:
    url: api.kr.datasaker.io
EOF
```
