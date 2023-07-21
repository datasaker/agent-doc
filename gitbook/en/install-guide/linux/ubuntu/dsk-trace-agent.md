# dsk-trace-agent

`Trace agent` works with open source distributed tracing systems such as opentelemetry and Jaeger to collect application distributed tracing data.
This allows you to trace communication between the various services inside your application, identify performance bottlenecks and optimize them.
Collected data is quickly processed and can be monitored and analyzed in real time.
We will adjust the "Trace Agent" settings to suit your requirements to provide optimal results.

# Did you run the DataSaker predecessor?

In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first.
wish. [DataSaker predecessor](${PREPARATION_MANUAL_KR})

# Install trace agent

## 1. Install the package

<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->

``` shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-trace-agent
```

## 2. Trace agent settings

``` shell
vi /etc/datasaker/dsk-trace-agent/agent-config.yaml
```

Modify the following as needed.

``` yaml
# Trace agent configuration file
agent:
  agent_name: "your_agent_name_what_you_want" # default=trace-agent
```

## 3. Run the package

```shell
systemctl enable dsk-trace-agent --now
```

## 4. Check Package Execution Status

```shell
sudo systemctl status dsk-trace-agent
```

# Trace agent port information

| Port | Protocol | Describe |
|---------|---------|----------------|
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

# Linking Trace Agent to Application

To use the Trace agent, you must first instrument your application with the SDK. Please see the link for more details.
[link to related documentation](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)

# Set trace agent

If necessary, you can change the settings of the trace agent by referring to the document below.

## Trace agent settings

The meanings and default values ​​of Trace agent settings are as follows. Different users have different requirements for agent setup. Therefore, the agent settings must be adjusted to suit the user's settings. for optimal results
Tune your agent settings. The agent preference file path is `/etc/datasaker/dsk-trace-agent/agent-config.yaml`.
Add or edit values ​​in the config file.

The structure of the file is as follows.

### `agent-config.yml`

```yaml
agent:
  # Agent's metadata
  metadata: <metadata>
  # Agent execution related options
  options:
    [ collector_config: <collector_config> ]
    [ receiver_config: <reciever_config> ]
```

#### `metadata`

```yaml
# agent name (alias)
[ agent_name: <string> | default = "dsk-trace-agent" ]

# Settings for which clusters the environment to be controlled are grouped into
[ cluster_id: <cluster_id> | default = "unknown" ]
```

#### `collector_config`

```yaml
# Sampling rate applied to the collector
# All data is collected when above 100
[ sampling_rate: <float> | default = 1 ]
```

#### `reciever_config`

```yaml
# Port number to receive data from the collector
[ listen_port: <uint16> | default = 14251 ]

# Custom tags applied to each span
[ custom_tags: <map[string]string> | default = "" ]
```

### Example
```yaml
agent:
  metadata:
    agent_name: dsk-trace-agent # agent name (alias) default=dsk-trace-agent
  options:
    collector_config:
      sampling_rate: 1 # span data sampling rate (0~100) default=1
```

# Remove trace agent

## 1. Package abort

```shell
systemctl stop dsk-trace-agent
```

## 2. Remove packages

```shell
sudo apt remove dsk-trace-agent
```