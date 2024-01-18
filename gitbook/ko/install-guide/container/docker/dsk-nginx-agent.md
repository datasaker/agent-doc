# dsk-nginx-agent
## 도커 환경에 DataSaker Nginx agent 설치하기
`Nginx agent`는 DataSaker에서 Nginx 정보를 수집하는 agent입니다.

## DataSaker 선행 작업을 진행하였나요?
현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Nginx agent 설치하기
### 1. nginx_status 활성화
nginx 설정 파일에 stub_status 설정을 추가합니다. 파일을 수정하고 nginx를 reload하면 nginx의 상태를 확인하고 Metric을 생성할 수 있습니다.
```
server {
    listen       80;
    server_name  localhost;
    
    # ... added part ...
    location /stub_status {
        stub_status on;
        allow all;
    }
    # ... skip ...
}
```
위 설정을 적용하기 위해 nginx를 reload 합니다.
```bash
$ sudo nginx -s reload
```

nginx를 reload하면 다음과 같이 stub_status를 확인할 수 있습니다.
```bash
$ curl http://localhost/stub_status

Active connections: 1
server accepts handled requests
 2 2 2
Reading: 0 Writing: 1 Waiting: 0
```

터미널에 다음 명령어를 입력하여 dsk-nginx-agent 설정 파일을 생성합니다.
```shell
cd ~
mkdir .datasaker
cat << EOF > ~/.datasaker/nginx-config.yml
agent:
  metadata:
    agent_name: dsk-nginx-agent
  option:
    exporter_config:
      command: "/etc/datasaker/target-exporter"
      args:
        - --nginx.scrape-uri=http://localhost:8080/stub_status
    scrape_interval: 15s
    scrape_timeout: 5s
    scrape_configs:
      - job_name: dsk-nginx-agent
        url: localhost:19113
        filtering_configs:
          rule: drop
EOF
```

### 2. Nginx agent 설치
Nginx agent를 설치합니다.
```shell
docker run -d --name dsk-nginx-agent \
    -v /var/datasaker/:/var/datasaker/ \
    -v ~/.datasaker/nginx-config.yml:/etc/datasaker/dsk-nginx-agent/agent-config.yml:ro \
    -e DSK_LOG_LEVEL=INFO \
    --restart=always \
    datasaker/dsk-nginx-agent
```



