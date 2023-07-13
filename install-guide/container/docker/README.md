# Docker 환경에서 DataSaker 설정파일 구성하기

## 사용자 정보 등록하기

`DataSaker` 설정파일 구성하기 위한 환경을 구성하고, 사용자 테넌트 정보를 등록합니다.

서버의 터미널을 열어 다음 명령어를 입력합니다.

```shell
sudo groupadd -g 202306 datasaker
sudo useradd -r -u 202306 -g datasaker -s /usr/sbin/nologin datasaker

mkdir -p ~/.datasaker
chmod 755 ~/.datasaker

cat << EOF > ~/.datasaker/config.yml
global:
  api_key: ${VAR_GLOBAL_APIKEY}
  gates:
    trace_datagate:
      url: gate.kr.datasaker.io:31300
    manifest_datagate:
      url: gate.kr.datasaker.io:31301
    metric_datagate:
      url: gate.kr.datasaker.io:31302
    plan_datagate:
      url: gate.kr.datasaker.io:31303
    loggate:
      url: gate.kr.datasaker.io:31304
  agent_manager:
    url: api.kr.datasaker.io
EOF
```
