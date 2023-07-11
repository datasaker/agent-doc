# Installing DataSaker Log Agent on Kubernetes with Helm

## Did you install Base Agent?

If you haven't installed **Base Agent**, please install **Base Agent** first. (Link)

## Edit the values.yaml file from ~/datasaker/agent-helm/values.yaml

Remove the square brackets after 'list:' in the 'list:' line.
Remove comments beside '-name;' and enter the information of log collection settings.

```shell
  vim ~/datasaker/agent-helm/values.yaml
```
Edit logAgent

```yaml
logAgent:
  isEnable: true
  imgPolicy: 'Always'
  imgVersion: 'latest'
  logLevel: 'INFO'
  tolerations: []
  resources:
    requests:
      cpu: 100m
      memory: 512Mi
    limits:
      cpu: 1
      memory: 2G
  environment: kubernetes
  collect:
    - paths:
        - /var/log/containers/postgresql*.log
      exclude_paths: []
      keywords: []
      tag: postgres_log
      service:
        name: postgres
        kind: my_pg
      source:
        type: database
        kind: postgres
        address: 0.0.0.0:5432
```

### Strategy of Agent Deployment

Define agent deployment settings to `toleration` section in '~/datasaker/agent-helm/values.yaml' file. For a detailed explanation of toleration, check [Official Kubernetes Document Link](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/).

## Helm Upgrade

```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

## Arguments value

Argument list for the **Datasaker log agent**.

| Argument             | Description                            |                    Default                    |
|----------------------|----------------------------------------|:---------------------------------------------:|
| `global.config`      | global config yaml file path           |       /etc/datasaker/global-config.yml        |
| `agent.config`       | agent config yaml file path            | /etc/datasaker/dsk-log-agent/agent-config.yml |
| `transport.tls.ca`   | TLS CA certification pem file path     |        /etc/datasaker/cert/ca-cert.pem        |
| `transport.tls.cert` | TLS client certification pem file path |      /etc/datasaker/cert/client-cert.pem      |
| `transport.tls.key`  | TLS client private key pem file path   |      /etc/datasaker/cert/client-key.pem       |

