# dsk-trace-agent

## Install Datasaker Trace agent in Docker environment

`Trace agent` works with open source distributed tracing systems such as `Opentelemetry` and `Jaeger` to collect application distributed tracing data. This allows you to trace communication between the various services inside your application, identify performance bottlenecks and optimize them. Collected data is quickly processed and can be monitored and analyzed in real time. We provide optimal results by adjusting the 'Trace agent' settings according to the customer's requirements.

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Docker environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessors] (README.md)

## Install Trace agent

Trace agent is using the following ports internally.

| Port | Protocol | Describe |
| ----- | -------- | -------------- |
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

You can deploy the Trace agent by exposing the required ports above.\
For example, when receiving only otlp data, it could be distributed like this:
```shell
     docker run -d --name dsk-trace-agent\
       -v /var/datasaker/:/var/datasaker/\
       -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
       -e DKS_LOG_LEVEL=info\
       -p 4317:4317/tcp\
       -p 4318:4318/tcp\
       --restart=always\
       datasaker/dsk-trace-agent
```
## Configure Trace agent

If necessary, you can change the settings of the trace agent by referring to the document below.

### Trace agent settings

The meaning of default value of trace agent setting value is as follows. Different users have different requirements for agent setup. Therefore, the agent settings must be adjusted to suit the user's settings. After adjusting the agent settings for optimal results, mount and use the configuration file as a volume when deploying the agent. The agent preference file path is `/etc/datasaker/dsk-trace-agent/agent-config.yml`. Add or edit values ​​in the config file.

The structure of the file is as follows.

#### `agent-config.yml`
```yaml
agent:
  metadata: <metadata>
  option:
    [ collector_config: <collector_config> ]
  [ reciever_config: <reciever_config> ]
```
**`metadata`**
```yaml
[ agent_name: <string> | default = "dsk-trace-agent" ]

[ cluster_id: <cluster_id> | default = "unknown" ]
```
**`collector_config`**
```yaml
[ sampling_rate: <float> | default = 1 ]
```
**`receiver_config`**
```yaml
[ listen_port: <uint16> | default = 14251 ]

[ custom_tags: <map[string]string> | default = "" ]
```
## Connecting Trace Agent to Application

For integration, the target application needs to set the following environment variables.

| environment variable | value | Description |
| -------------------------------------- | ---------------------------------------------- | ------------------ |
| OTEL\_EXPORTER\_OTLP\_TRACES\_ENDPOINT | (trace-agent):(port) | trace-agent address |
| OTEL\_SERVICE\_NAME | your-service-name | The name of the service you want displayed on the screen |
| OTEL\_METRICS\_EXPORTER | none | For preventing unnecessary metric data creation |
| OTEL\_LOGS\_EXPORTER | none | For preventing unnecessary log data creation |
| OTEL\_RESOURCE\_ATTRIBUTES | dsk\_host\_key=$(cat /var/datasaker/host\_key) | - |

For example, you can distribute your application like this:
```shell
docker run my-java-application \
    -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=<trace-agent-address>:4317 \
    -e OTEL_SERVICE_NAME=your-service-name \
    -e OTEL_METRICS_EXPORTER=none \
    -e OTEL_LOGS_EXPORTER=none \
    -e OTEL_RESOURCE_ATTRIBUTES=dsk_host_key=$(cat /var/datasaker/host_key) \
    -d
```
For more detailed information, see:

[link to related documentation](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)