# Installing DataSaker Elasticsearch Agent on Kubernetes with helm

## Did you install the Base Agent?

If you haven't installed **Base Agent**, please install **Base Agent** first. (Link)

## ~/datasaker/agent-helm/values.yaml modification

Remove the square brackets after 'list:' in the 'list:' line.
Remove comments beside '-name;' and enter the information of elasticsearch.

``` shell
  vim ~/datasaker/agent-helm/values.yaml
```

```yaml
elasticsearchAgent:
  tolerations: []
  list: #[] # lines, adjust them as necessary, and remove the square brackets after 'list:'.
    - name: 'es-1'
      imgPolicy: 'Always'
      imgVersion: 'develop'
      logLevel: 'DEBUG'
      extraArgs:
        - --es.uri=https://es_id:es_pw@elasticsearch_service.namespace.svc.cluster.local:9200
        - --es.all=true
        - --es.indices=true
        - --es.aliases=true
        - --es.shards=true
      extraEnv:
        - name: ENV_1
          value: ENV_1
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
```

### Strategy of Agent Deployment

Define agent deployment settings to `toleration` section in '~/datasaker/agent-helm/values.yaml' file. For a detailed explanation of toleration, check [Official Kubernetes Document Link](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/).

## Update the helm chart

```shell
  helm upgrade datasaker ~/datasaker/agent-helm -f ~/datasaker/agent-helm/values.yaml
```

## Arguments for the DataSaker Elasticsearch Agent

You can add the settings for the **DataSaker Elasticsearch Agent**.

Check the [DataSaker Elasticsearch Agent Settings](../../../../../settings/dsk-elasticsearch-agent/settings.md) for more information.
