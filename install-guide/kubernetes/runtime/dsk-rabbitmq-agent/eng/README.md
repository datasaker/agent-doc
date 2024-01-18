# dsk-rabbitmq-agent (english)
## Install DataSaker RabbitMQ agent in Kubernetes
`RabbitMQ agent` is an agent that collects RabbitMQ information from DataSaker.

## Have you done DataSaker pre-work?
If you have not done the pre-work of `DataSaker` in the current Kubernetes environment, please do the pre-work of `DataSaker` first. [DataSaker pre-work](README.md)

# Install RabbitMQ agent
### 1. Register RabbitMQ agent configuration value
Please register rabbitmq information in rabbitmqAgent.list[].
```shell
cat << EOF >> ~/datasaker/config.yaml

rabbitmqAgent:
  list:
    - name: 'rabbitmq-1'
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      resources: {}
      tolerations: []
      extraArgs: []
      extraEnvs: []
      rabbitmqAddr: "http://localhost:15672"
      rabbitmqUser: "root"
      rabbitmqPass: "root"
      rabbitmqCA: ""
      rabbitmqCert: ""
      rabbitmqKey: ""
      listenPort: 19419
EOF
```

The following is a detailed description of each setting value.

| Setting | Description |
|--------------|-------------------|
| name | agent name |
| imgPolicy | Kubernetes image policy setting |
| imgVersion | Agent image version setting |
| logLevel | Agent log level setting |
| resources | Agent Kubernetes resource setting |
| tolerations | Kubernetes toleration setting |
| extraArgs | Agent additional option setting |
| extraEnvs | Agent additional environment variable setting |
| rabbitmqAddr | rabbitmq address |
| rabbitmqUser | rabbitmq user |
| rabbitmqPass | rabbitmq password |
| rabbitmqCA | rabbitmq CA certificate |
| rabbitmqCert | rabbitmq certificate |
| rabbitmqKey | rabbitmq certificate key |
| listenPort | Agent port setting |

### 2. Install RabbitMQ agent
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

