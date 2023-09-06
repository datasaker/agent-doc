# dsk-trace-agent

`trace-agent` works with open source distributed tracing systems such as opentelemetry and Jaeger to collect distributed tracing data of applications.
This allows you to trace communication between the various services inside your application, identify performance bottlenecks and optimize them.
Collected data is quickly processed and can be monitored and analyzed in real time.
We will adjust the "Trace Agent" settings to suit your requirements to provide optimal results.

## Have you done the Datasaker predecessor?

If the preceding task of `DataSaker` has not been performed in the current environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](README.md)

## Install Trace agent

### 1. Install the package
```shell
yum install dsk-trace-agent
```
### 2. Trace agent setting
```shell
vi /etc/datasaker/dsk-trace-agent/agent-config.yml
```
Modify the following as needed.
```yaml
agent:
  agent_name: "dsk-trace-agent"
```
### 3. Run the package
```shell
systemctl enable dsk-trace-agent --now
```
### 4. Check package execution status
```shell
systemctl status dsk-trace-agent
```
## Trace agent port information

| Port | Protocol | Describe |
|---------|---------|----------------|
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

## Integrating the trace agent into the application

To use the Trace agent, you must first instrument your application with the SDK.
Please see the link for more details.
[link to related documentation](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)

## Configure Trace agent

If necessary, you can change the settings of the trace agent by referring to the document below.

### Trace agent settings

The meaning of default value of trace agent setting value is as follows.
Different users have different requirements for agent setup.
Therefore, the agent settings must be adjusted to suit the user's settings.
Tune your agent settings for optimal results.
The agent preference file path is `/etc/datasaker/dsk-trace-agent/agent-config.yml`.
Add or edit values ​​in the config file.

The structure of the file is as follows.

### `agent-config.yml`
```yaml
agent:
  metadata: <metadata>
  option:
    [ collector_config: <collector_config> ]
  [ reciever_config: <reciever_config> ]
```
#### `metadata`
```yaml
[ agent_name: <string> | default = "dsk-trace-agent" ]

[ cluster_id: <cluster_id> | default = "unknown" ]
```
A detailed explanation of each setting value is as follows.

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent_name` | Set the agent's name. | dsk-trace-agent | No |
| `cluster_id` | Set the cluster to which the environment to be monitored is grouped. | unknown | No |

#### `collector_config`
```yaml
[ sampling_rate: <float> | default = 1 ]
```
A detailed description of each setting follows.

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `sampling_rate` | Sets the sampling rate applied to the collector. When above 100 all data is collected. | 1 | No |

#### `receiver_config`
```yaml
[ custom_tags: <map[string]string> | default = "" ]
```
A detailed description of each setting follows.

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | ----------- | ------------ |
| `listen_port` | Set the port number to receive data from the collector. | 14251 | No |
| `custom_tags` | Set custom tags applied to each span. | "" | No |

### Example
```yaml
agent:
  metadata:
    agent_name: dsk-trace-agent 
  option:
    collector_configs:
    sampling_rate: 1
```
## Remove trace agent

### 1. Abort the package
```shell
systemctl stop dsk-trace-agent
```
### 2. Remove packages
```shell
yum remove dsk-trace-agent
```
