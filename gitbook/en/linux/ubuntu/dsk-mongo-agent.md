# dsk-mongo-agent

# Did you run the DataSaker predecessor?
In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](${PREPARATION_MANUAL_KR})

# Install Mongo agent
## 1. Install the package
To install the `Mongo agent` of `DataSaker`, sudo permission is required.
<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mongo-agent
```

## 2. Mongo agent configuration

```shell
vi /etc/datasaker/dsk-mongo-agent/agent-config.yml
```

Modify the following as needed.

```yaml
# Base agent configuration file
agent:
  agent_name: "dsk-mongo-agent" # agent name (alias) default=dsk-mongo-agent
```

## 3. Run the package

```bash
systemctl start dsk-mongo-agent
```

## 4. Check Package Execution Status

```bash
systemctl status dsk-mongo-agent
```

or

```bash
service dsk-mongo-agent status
```

### How to set up the package

You may need to change the agent configuration to monitor MongoDB. \

### Args list of arguments

`DataSaker` allows you to add settings for the Mongo agent.
Please refer to the following document. [Related Documents](../../../../../settings/dsk-mongo-agent/settings.md)

---
# Remove the Mongo agent

## 1. Package abort

```bash
systemctl stop dsk-mongo-agent
```

or

```bash
service dsk-mongo-agent stop
```

## 2. Remove packages

```bash
sudo apt remove dsk-mongo-agent
```