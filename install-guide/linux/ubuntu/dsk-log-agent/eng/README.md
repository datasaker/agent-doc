# Coming soon

<!--
# Install Log agent on Ubuntu/Debian

## global-config Configuration
If `/etc/datasaker/global-config.yml` does not exist, configure `global-config.yml` as below.

**global-config.yml**
```bash
echo "
global:
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
    send_interval: 1m
" > /etc/datasaker/global-config.yml
```

## agent-config Configuration

```yaml
agent:
  metadata:
    agent_name:         # Agent Name
    cluster_id:         # Cluster Information
  environment:          # Log Collect Environment [kubernetes, docker, etc]
  collect:
    - paths: []         # Log path to collect
      exclude_paths: [] # Exclude Log path from the 'paths'
      keywords: []      # Log collect keyword (When keywords are set, log messages containing the keywords are collected)
      tag:              # User custom tag
      service:
        - name:         # service name 
        - kind:         # service kind
      source:
        - type:         # Log source type [app, database, syslog, etc]
        - kind:         # Log source kind [postgres, mysql, java, python, go]
        - address:      # User custom settings - DB host and port information
```

## 1. Installing the package

> sudo permission is required.

```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/qPwDBm8QRLf7PGA/download/dsk-log-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-log-agent init "'${DSK_GLOBAL_APIKEY}'" && sudo /usr/bin/dsk-log-agent start'
```

## 2. Starting package

```bash
$ sudo dsk-log-agent start
```

> Warning
> 1. To enable log collection, you need to set up configuration files(`global-config.yml`, `config.conf`) before starting package.
> 2. If you are already using the Log agent default port, you can change it via ENV(`FLUENTD_RELOAD_PORT`, `FLUENTD_HEALTHCHECK_PORT`).

## 3. Checking the status of package

```bash
$ sudo dsk-log-agent status
fluentd is running
Agent is running
```

## 4. Stopping the package

```bash
sudo dsk-log-agent stop
```

## 5. Uninstalling the package

```bash
sudo apt-get remove dsk-log-agent
```
-->