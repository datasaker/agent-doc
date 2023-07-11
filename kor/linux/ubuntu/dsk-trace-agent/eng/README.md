# Coming soon

<!--
# Installing DataSaker Trace agent on Ubuntu

`Trace agent` works with open source distributed tracing systems such as opentelemetry and Jaeger to collect application distributed tracing data.
This allows you to trace communication between the various services inside your application, identify performance bottlenecks and optimize them.
Collected data is quickly processed and can be monitored and analyzed in real time.
We will adjust the "Trace Agent" settings to suit your requirements to provide optimal results.

## Did you create a DataSaker configuration file?

In the current Ubuntu environment, a basic configuration file must be created to install the `DataSaker` agent. If you have not created a default configuration file, please create one. [DataSaker 설정하기](../../README.md)

# Install Trace agent
## 1. Installing the package
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-trace-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-trace-agent init "'${DSK_GLOBAL_APIKEY}'"'
```

## 2. Setting the configuration file
``` bash
vi /etc/datasaker/dsk-trace-agent/agent-config.yaml
```

``` yaml
# Trace agent 설정 파일
agent:
  agent_name: "your_agent_name_what_you_want" # default=trace-agent
  cluster_id: "test-cluster-id"               # default=unknown_cluster
```

## 3. Run the package
```bash
sudo /usr/bin/dsk-trace-agent start
```

## 4. Checking the status of package
```bash
$ sudo dsk-trace-agent status
Agent is running
Exporter is running
```

# Uninstall Trace agent
## 1. Stopping the package
```bash
sudo dsk-trace-agent stop
```

## 2. Uninstalling the package
```bash
sudo apt-get remove dsk-trace-agent
```
-->