# Installing DataSaker PostgreSQL Agent on Kubernetes with Helm

## Did you install Base Agent?

If you haven't installed **Base Agent**, please install **Base Agent** first. (Link)

## Edit the values.yaml file from ~/datasaker/agent-helm/values.yaml

Remove the square brackets after 'list:' in the 'list:' line.
Remove comments beside '-name;' and enter the information of PostgreSQL Database you want to monitor.

```shell
  vim ~/datasaker/agent-helm/values.yaml
```

Edit postgresAgent.list

```yaml
postgresAgent:
  tolerations: []
  list: #[] <- Remove the square brackets after 'list:'. or comment out the brackets.
    - name: 'pg-1'
      targetAddr: '127.0.0.1:5432'
      database: "postgres"
      user: 'user'
      pass: "pass"
      imgPolicy: 'Always'
      imgVersion: 'develop'
      logLevel: 'DEBUG'
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

## Helm Upgrade

```shell
helm upgrade datasaker ~/datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
