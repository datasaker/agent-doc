# Configure Datasaker configuration file in Kubernetes environment

`DataSaker` proceeds with installation guide through `Helm` in `Kubernetes` environment. (If `Helm` is not installed, please install Helm first. https://helm.sh/en/docs/intro/install/)

## Register user information

Register user tenant information to configure `DataSaker` configuration file.

### create directory

First, configure the environment to configure the `DataSaker` configuration file.
```shell
mkdir -p ~/datasaker
chmod 755 ~/datasaker
```
### Create user information registration file

Create a user information registration file by referring to the following command.
```shell
cat << EOF > ~/datasaker/config.yaml
userInfo:
  clusterName: "<YOUR_CLUSTER_NAME>"
  apiKey: "<YOUR_API_KEY>"
  runtimeType: "<YOUR_RUNTIME_TYPE>"
EOF
```
Each piece of information means:

| information | Description |
| ----------- | ---------------------------------------- |
| clusterName | User-provided Kubernetes cluster name. |
| apiKey | API Key issued by DataSaker. |
| runtimeType | Kubernetes runtime type. (docker, containerd, crio) |

## Configure DataSaker configuration file using Helm

### Add DataSaker Helm Repository
```shell
helm repo add datasaker https://datasaker.github.io/agent-helm/
```
### Create DataSaker configuration file through Helm install
```shell
helm install datasaker datasaker/agent-helm -n datasaker --create-namespace \
  -f ~/datasaker/config.yaml
```
When adding a new chart, please proceed with `helm repo update`.