# dsk-trace-agent

`Trace agent` works with open source distributed tracing systems such as opentelemetry and Jaeger to collect application distributed tracing data.
This allows you to trace communication between the various services inside your application, identify performance bottlenecks and optimize them.
Collected data is quickly processed and can be monitored and analyzed in real time.
We will adjust the "Trace Agent" settings to suit your requirements to provide optimal results.

# Did you run the Datasaker predecessor?
If the preceding task of `DataSaker` has not been carried out in the current RedHat 7 environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](${PREPARATION_MANUAL_KR})

# Install trace agent
## 1. Install the package
```shell
sudo yum install dsk-trace-agent
```

## 2. Trace agent settings
```shell
sudo vi /etc/datasaker/dsk-trace-agent/agent-config.yml
```
Modify the following as needed.

```yaml
# Trace agent configuration file
agent:
  metadata:
    agent_name: "dsk-trace-agent" # agent name (alias) default=dsk-trace-agent
```

## 3. Run the package
```shell
sudo systemctl enable dsk-trace-agent --now
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

# Integrating the trace agent into the application
To use the Trace agent, you must first instrument your application with the SDK.
Please see the link for more details.
[link to related documentation](https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation)

# Set trace agent
If necessary, you can change the settings of the trace agent by referring to the document below.

## Trace agent settings
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

#### `receiver_config`
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
    collector_configs:
      sampling_rate: 1
```

# Remove trace agent
## 1. Package abort
```shell
sudo systemctl stop dsk-trace-agent
```

## 2. Remove packages
```shell
sudo systemctl stop dsk-trace-agent &&
sudo yum remove dsk-trace-agent
```