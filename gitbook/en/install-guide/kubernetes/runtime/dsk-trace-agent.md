# dsk-trace-agent

## Install DataSaker Trace agent in Kubernetes environment

`Trace agent` works with open source distributed tracing systems such as `Opentelemetry` and `Jaeger` to collect application distributed tracing data. This allows you to trace communication between the various services inside your application, identify performance bottlenecks and optimize them. Collected data is quickly processed and can be monitored and analyzed in real time. We provide optimal results by adjusting the 'Trace agent' settings according to the customer's requirements.

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Kubernetes environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessors] (README.md)

## Install Trace agent

### 1. Register Trace agent settings
```shell
cat << EOF >> ~/datasaker/config.yaml

traceAgent:
  enabled: true
  logLevel: 'INFO'
EOF
```
### 2. Trace agent installation
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
## Trace agent port information

| Port | Protocol | Describe |
| ----- | -------- | -------------- |
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

## Configure Trace agent

### Trace agent settings

The meanings and default values ​​of Trace agent settings are as follows. Different users have different requirements for agent setup. Therefore, the agent settings must be adjusted to suit the user's settings. Tune your agent settings for optimal results. Add or edit these values ​​in "\~/datasaker/config.yaml". The value set for each item is the default setting value.
```yaml
traceAgent:
  enabled: false
  tolerations: []
  imgPolicy: 'Always' # [Always, IfNotPresent, Never]
  imgVersion: 'latest'
  tags: []
  resource:
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1000m
      memory: 1000Mi
  logLevel: 'INFO'
  nodeSelector: {}
  affinity: {}
  collector:
    samplingRate: 1
```
A detailed explanation of each setting value is as follows.

| set value | Description |
| ----------- | ----------------------------------------------------------------------- |
| enabled | Enable trace agent. |
| tolerations | Add a toleration setting if a taint is set on the worker node. |
| imgPolicy | Set the agent's Image Policy. (Always, IfNotPresent, Never) |
| imgVersion | Set the Image Version of the agent. |
| logLevel | Set the log level of the agent. (debug > info > warn > error > panic > fatal) |
| tags | Enter the tag list to be added to the data as a user setting. key1=value1,key2=value2 |
| resource | Set the agent's resource. If it is too small, normal operation may not be possible. |

## Connecting Trace Agent to Application

[link to related documentation](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)