# dsk-base-agent

`base-agent` collects various information from the server in real time.
For example, you can collect various information such as server performance indicators such as memory and CPU usage, network traffic, and container information.
Through this, customers can monitor the server's status in real time, and based on this, optimize the server's performance and increase stability.
We tailor agent settings to your needs to deliver optimal results.

## Have you done the Datasaker predecessor?

If the preceding task of `DataSaker` has not been performed in the current environment, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](README.md)

## Install the base agent

### 1. Install the package

sudo permission is required to install `base-agent` of `DataSaker`.
<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-node-agent
```
## 2. Base agent setup
```shell
vi /etc/datasaker/dsk-node-agent/agent-config.yml
```
Modify the following as needed.
```yaml
agent:
  agent_name: "dsk-base-agent"
  cluster_id: "my-cluster-id"
```
A description of each setting follows.

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent_name` | Agent Name (Alias) | dsk-base-agent | No |
| `cluster_id` | Set the cluster to which the environment to be monitored is grouped. | unknown | No |

### 3. Run the package
```shell
systemctl enable dsk-node-agent --now
```
### 4. Check package execution status
```shell
systemctl status dsk-node-agent
```
## Remove the base agent

### 1. Abort the package
```shell
systemctl stop dsk-node-agent
```
### 2. Remove packages
```bash
sudo apt remove dsk-node-agent
```
