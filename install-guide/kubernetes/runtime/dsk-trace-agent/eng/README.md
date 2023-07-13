# Installing DataSaker Trace agent on Kubernetes

Trace agent is an agent that collects trace information.

## Did you install Base Agent?

If you haven't installed `DataSaker` on your Kubrenetes Cluster, please install `DataSaker` first. [Installing DataSaker](../../../README.md)

# Install Trace agent

## 1. Set the Trace agent configuration values

```shell
cat << EOF >> ~/datasaker/config.yaml

traceAgent:
  enabled: true
EOF
```

### Trace agent configuration values

Configurations for Trace agent are as follows. You can change the values according to your environment.
Edit the values in "~/datasaker/config.yaml" file.

```yaml
traceAgent:
  enabled: false                    # Enable or Disable Trace agent
  tolerations: []                   # Set Toleration if you want to deploy agent on specific node.
  imgPolicy: Always                 # Set agent's imagePullPolicy (Always, IfNotPresent, Never)
  imgVersion: 'latest'              # Set agent's image version
  logLevel: 'INFO'                  # Set agent's log level (debug > info > warn > error > panic > fatal)
  tags: 'key1=value1,key2=value2'   # Set Custom tags if you want to add custom tags to the agent.
  resource:                         # Set agent's resource limit (CPU, Memory) 
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1000m
      memory: 1000Mi
```
<!--
  nodeSelector: {}                  # (option) set nodeSelector if you want to deploy agent on specific node.
  affinity: {}                      # (option) set affinity if you want to deploy agent on specific node.
  collector:
    samplingRate: 10                # (option) set Trace data sampling rate. (0 < sampleRate <= 100)
```
-->

## 2. Run Trace agent

```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

# Trace agent port information

| Port  | Protocol | Describe       |
|-------|----------|----------------|
| 6831  | UDP      | thrift-compact |
| 6832  | UDP      | thrift-binary  |
| 14250 | TCP      | jaeger-grpc    |
| 14268 | TCP      | jaeger-http    |
| 4317  | TCP      | otlp-grpc      |
| 4318  | TCP      | otlp-http      |

<!--
# Caution

> By default, Trace agent is deployed as a daemonset. \
> Therefore, Trace agent is installed on all nodes. \
> If you want to install Trace agent only on specific nodes, set affinity or nodeSelector for the node. \
> However, opentelemetry-instrumented applications must be installed on the same node as the Trace agent.
-->
