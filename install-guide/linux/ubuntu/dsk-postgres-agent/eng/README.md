# Coming soon

<!--
# Install PostgreSQL agent on Ubuntu/Debian

## 1. Installing the package

> **sudo** permission is required.

```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-postgres-agent init "'${DSK_GLOBAL_APIKEY}'" && sudo /usr/bin/dsk-postgres-agent start'
```

## 2. Checking the status of package

```bash
$ sudo dsk-postgres-agents status
Agent is running
Exporter is running
```

## 3. Stopping the package

```bash
sudo dsk-postgres-agent stop
```

## 4. Uninstalling the package

```bash
sudo apt-get remove dsk-postgres-agent
```
-->