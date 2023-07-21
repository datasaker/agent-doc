# Configure Datasaker configuration file in RedHat 8 environment

## Registering the yum repository
```shell
cat <<EOF | sudo tee /etc/yum.repos.d/datasaker.repo
[datasaker]
name=datasaker-repo
baseurl=http://nexus.exem-oss.org/repository/datasaker-redhat-8
enabled=1
gpgcheck=0
EOF
```

## Configuring global-config.yml
```shell
sudo mkdir -p /etc/datasaker

sudo touch /etc/datasaker/global-config.yml

echo 'global:
  api_key: ${VAR_GLOBAL_APIKEY}
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