# Configure datasaker configuration file in Ubuntu environment

## Configuring global-config.yml

First, configure the environment to configure the `DataSaker` configuration file.
```shell
sudo mkdir -p /etc/datasaker

sudo touch /etc/datasaker/global-config.yml
```
Then configure the configuration file using the API Key issued from `Datasaker`.
```shell
echo 'global:
  api_key: "<YOUR_API_KEY>"
  gates:
    metric_datagate:
      url: gate.kr.datasaker.io:31302
      remote_timeout: 5s
    manifest_datagate:
      url: gate.kr.datasaker.io:31301
      remote_timeout: 5s
    trace_datagate:
      url: gate.kr.datasaker.io:31300
      remote_timeout: 5s
    plan_datagate:
      url: gate.kr.datasaker.io:31303
      remote_timeout: 5s
    loggate:
      url: gate.kr.datasaker.io:31304
      remote_timeout: 5s
  agent_manager:
    url: api.kr.datasaker.io
    base_url: /dsk-agentmanager-api/agent
    send_interval: 1m' | sudo tee /etc/datasaker/global-config.yml
```
