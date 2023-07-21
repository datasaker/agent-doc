# Ubuntu 환경에 데이터세이커 설정파일 구성하기

## global-config.yml 구성하기

먼저, `DataSaker` 설정파일을 구성하기 위한 환경을 구성합니다.

```shell
sudo mkdir -p /etc/datasaker

sudo touch /etc/datasaker/global-config.yml
```

이후 `Datasaker`에서 발급받은 API Key를 사용하여 설정 파일을 구성합니다.

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
