＃Amazon Linux 2023環境でDatasaker設定ファイルを設定する

## yum repositoryを登録する
```shell
cat <<EOF | sudo tee /etc/yum.repos.d/datasaker.repo
[datasaker]
name=datasaker-repo
baseurl=http://nexus.exem-oss.org/repository/datasaker-amazonlinux-2023
enabled=1
gpgcheck=0
EOF
```
## global-config.ymlの設定

まず、`DataSaker`設定ファイルを設定するための環境を設定します。
```shell
sudo mkdir -p /etc/datasaker

sudo touch /etc/datasaker/global-config.yml
```
その後、 `Datasaker`から発行されたAPI Keyを使用して設定ファイルを設定します。
```shell
echo 'global:
  api_key: "<YOUR_API_KEY>"
  gates:
    metric_datagate:
      url: gate.kr.datasaker.io:31302
      remote_timeout: 5s
    manifest_datagate:
      url: gate.kr.datasaker.io:31301
      remote_timeout: 5s
    trace_datagate:
      url: gate.kr.datasaker.io:31300
      remote_timeout: 5s
    plan_datagate:
      url: gate.kr.datasaker.io:31303
      remote_timeout: 5s
    loggate:
      url: gate.kr.datasaker.io:31304
      remote_timeout: 5s
  agent_manager:
    url: api.kr.datasaker.io
    base_url: /dsk-agentmanager-api/agent
    send_interval: 1m' | sudo tee /etc/datasaker/global-config.yml
```
