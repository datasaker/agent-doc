# dsk-elasticsearch-agent

## Installing DataSaker Elasticsearch agent in Kubernetes environment

`Elasticsearch agent` is an agent that collects elasticsearch information from DataSaker.

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Kubernetes environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](dsk-elasticsearch-agent/en/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Install the Elasticsearch agent

### 1. Register Elasticsearch agent settings

Be sure to register elasticsearch addresses information in elasticsearchAgent.list\[].uri
```shell
cat << EOF >> ~/datasaker/config.yaml

elasticsearchAgent:
  list:
    - name: 'es-1'
      uri: 'https://es_id:es_pw@elasticsearch_service.namespace.svc.cluster.local:9200'
      tolerations: []
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19114
      exporterArgs:
        - --es.timeout=5s
      extraArgs: []
      extraEnvs:
        - name: ENV_1
          value: ENV_1
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
```
A detailed explanation of each setting value is as follows.

| settings | Description |
| ------------ | ----------------- |
| name | agent's name |
| uri | elasticsearch address |
| tolerations | Kubernetes Toleration Settings |
| imgPolicy | Kubernetes image policy settings |
| imgVersion | Set agent image version |
| logLevel | Set agent log level |
| listenPort | Agent Port Settings |
| exporterArgs | Set agent options |
| extraArgs | Set Agent Additional Options |
| extraEnvs | Agent Additional Environment Variable Settings |
| resources | Agent Kubernetes Resource Settings |

### 2. Install the Elasticsearch agent
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
For more detailed information, please refer to the following document. [Related Documents](../../../settings/dsk-elasticsearch-agent/settings.md)