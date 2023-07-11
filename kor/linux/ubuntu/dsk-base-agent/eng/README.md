# Coming soon

<!--
# Install Base agent on Ubuntu/Debian

## 1. Installing the package

> **sudo** permission is required.

```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-node-agent

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-node-agent init "'${DSK_GLOBAL_APIKEY}'" && systemctl enable dsk-node-agent --now'
```

## 2. Checking the status of package

```bash
systemctl status dsk-node-agent
```

or use `service` command

```bash
service dsk-node-agent status
```

## 3. Stopping the package

```bash
systemctl stop dsk-node-agent
```

or use `service` command

```bash
service dsk-node-agent stop
```

## 4. Uninstalling the package

```bash
sudo apt remove dsk-node-agent
```
-->