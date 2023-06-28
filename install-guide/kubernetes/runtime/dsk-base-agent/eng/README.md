# Installing Base Agent on Kubernetes with Helm chart

## Register User Tenant Information

**Datasaker** provides a Helm chart to install the **Base Agent** on Kubernetes. Before installing the chart, you need to register your user tenant information.

Make **datasaker** directory, and give 755 permission

```shell
mkdir -p ~/datasaker
chmod 755 ~/datasaker/
```

**Datasaker** user information registration

```shell
cat << EOF > ~/datasaker/config.yaml
userInfo:
  clusterName: ${VAR_CLUSTER_NAME}
  apiKey: ${VAR_GLOBAL_APIKEY}
  runtimeType: ${VAR_RUNTIME_TYPE}
EOF
```

## Install Base Agent with helm

- Add the **Datasaker** Helm repository.

  ```shell
  helm repo add datasaker https://datasaker.github.io/agent-helm/
  ```

- Pull **Datasaker** Helm

  ```shell
  helm pull datasaker/agent-helm
  ```

- Uzip helm chart

  ```shell
  tar -zxvf agent-helm-0.1.0.tgz -C ~/datasaker
  ```

- Install **Base Agent**

  ```shell
  helm install datasaker ~/datasaker/agent-helm -n datasaker --create-namespace \
    -f ~/datasaker/config.yaml
  ```
