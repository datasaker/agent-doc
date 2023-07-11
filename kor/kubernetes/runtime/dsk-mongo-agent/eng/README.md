# Install Mongo Agent with Helm in Kubernetes

## DId you install Base Agent?

If you haven't installed **Base Agent**, please install **Base Agent** first. (Link)

## Edit the values.yaml file from ~/datasaker/agent-helm/values.yaml

Remove the square brackets after 'list:' in the 'list:' line.
Remove comments beside '-name;' and enter the information of elasticsearch.

```shell
  vim ~/datasaker/agent-helm/values.yaml
```

```yaml
mongoAgent:
  tolerations: []
  list: # [] <- Remove the square brackets after 'list:'. or comment out the brackets.
    - name: 'mongo-1'
      targetAddr: '127.0.0.1:27017'
      user: 'user'
      pass: "password"
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'DEBUG'
      extraArgs:
        - --mongo.argument1
        - --mongo.argument2
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
    - name: 'mongo-2'
      targetAddr: '127.0.0.1:27017'
      user: 'user'
      pass: "password"
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'DEBUG'
      extraArgs: []
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

## Upgrade helm chart

```shell
  helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
    -f ~/datasaker/config.yaml
```
