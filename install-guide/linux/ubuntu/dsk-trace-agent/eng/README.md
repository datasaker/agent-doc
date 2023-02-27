# Install Trace agent on Ubuntu/Debian

## 1. Installing the package

> **sudo** permission is required.

<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/BTa8BGyckeCHKkC/download/dsk-trace-agent-install.sh
chmod 700 installer.sh
sudo ./installer.sh

sudo DSK_GLOBAL_APIKEY=${DSK_GLOBAL_APIKEY} bash -c '/usr/bin/dsk-trace-agent init "'${DSK_GLOBAL_APIKEY}'" && sudo /usr/bin/dsk-trace-agent start'
```

## 2. Checking the status of package

```bash
$ sudo dsk-trace-agents status
Agent is running
Exporter is running
```

## 3. Stopping the package

```bash
sudo dsk-trace-agent stop
```

## 4. Uninstalling the package

```bash
sudo apt-get remove dsk-trace-agent
```