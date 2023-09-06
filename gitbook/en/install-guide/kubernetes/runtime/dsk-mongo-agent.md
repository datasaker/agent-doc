# dsk-mongo-agent

## Install DataSaker Mongo agent in Kubernetes environment

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Kubernetes environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessors] (README.md)

## Install Mongo agent

### 1. Register Mongo agent settings
```shell
cat << EOF >> ~/datasaker/config.yaml

mongoAgent:
  list: []
EOF
```
#### Mongo agent settings

The meanings and default values ​​of the Mongo agent settings are as follows. Different users have different requirements for agent setup. Therefore, the agent settings must be adjusted to suit the user's settings. Tune your agent settings for optimal results. Add or edit these values ​​in "\~/datasaker/config.yaml".
```yaml
```
### 2. Install Mongo agent
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
