＃Ubuntu環境でデータセーカー設定ファイルを構成する

## global-config.ymlの設定

```shell
sudo mkdir -p /etc/datasaker

sudo touch /etc/datasaker/global-config.yml

echo 'global:
  api_key: ${VAR_GLOBAL_APIKEY}
  gates:
    metric_datagate:
      url:gate.jp.datasaker.io:31302
      remote_timeout: 5s
    manifest_datagate：
      url:gate.jp.datasaker.io:31301
      remote_timeout: 5s
    trace_datagate:
      url:gate.jp.datasaker.io:31300
      remote_timeout: 5s
    plan_datagate:
      url:gate.jp.datasaker.io:31303
      remote_timeout: 5s
    loggate:
      url:gate.jp.datasaker.io:31304
      remote_timeout: 5s
  agent_manager:
    url: api.jp.datasaker.io
    base_url: /dsk-agentmanager-api/agent
    send_interval：1m '| sudo tee /etc/datasaker/global-config.yml
```

\