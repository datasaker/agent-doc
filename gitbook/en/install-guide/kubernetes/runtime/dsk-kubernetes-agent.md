# dsk-kubernetes-agent

## Installing DataSaker Kubernetes agent in Kubernetes environment

The `Kubernetes agent` collects the state of your Kubernetes cluster in real time. This allows you to collect a variety of information, such as the health of nodes and pods in your cluster, resource usage, and events. In addition, by linking with the Kubernetes API server, you can detect creation events of cluster nodes and resources in real time, and respond appropriately when problems occur. By analyzing the collected data, you can contribute to improving the performance and strengthening the stability of the cluster. We tailor agent settings to your needs to deliver optimal results.

## Did you run the DataSaker predecessor?

If the preceding task of `DataSaker` has not been carried out in the current Kubernetes environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessors] (README.md)

## Install the Kubernetes agent

### 1. Register Kubernetes agent settings
```shell
cat << EOF >> ~/datasaker/config.yaml

kubernetesAgent:
  enabled: true
  kubeStateAgent:
    logLevel: 'INFO'
  k8sAgent:
    logLevel: 'INFO'
EOF
```
### 2. Install Kubernetes agent
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
