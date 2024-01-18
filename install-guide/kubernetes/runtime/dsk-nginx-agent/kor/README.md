# dsk-nginx-agent
## 쿠버네티스 환경에서 DataSaker Nginx agent 설치하기
`Nginx agent`는 DataSaker에서 Nginx 정보를 수집하는 agent입니다.

## DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Nginx agent 설치하기
### 1. nginx_status 활성화
nginx 설정 파일에 stub_status 설정을 추가합니다. 파일을 수정하고 nginx를 reload하면 prometheus가 nginx의 상태를 확인하고 Metric을 생성할 수 있습니다.
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

### 2. Nginx agent 설정값 등록
nginxAgent.list[]에 nginx 정보를 반드시 등록해주세요
```shell
cat << EOF >> ~/datasaker/config.yaml

nginxAgent:
  enabled: true
  tolerations: []
  imgPolicy: 'Always'
  imgVersion: 'latest'
  tags: []
  resource: {}
  logLevel: 'INFO'
  targetAddr: "http://localhost:8080/nginx_status"
  listenPort: 19113
EOF
```

각 설정 값에 대한 자세한 설명은 다음과 같습니다.

| 설정           | 설명                |
|--------------|-------------------|
| enabled      | agent 활성화 여부      |
| tolerations  | 쿠버네티스 톨러레이션 설정    |
| imgPolicy    | 쿠버네티스 이미지 정책 설정   |
| imgVersion   | 에이전트 이미지 버전 설정    |
| tags         | 에이전트 태그 설정        |
| resource     | 에이전트 쿠버네티스 리소스 설정 |
| logLevel     | 에이전트 로그 레벨 설정     |
| targetAddr   | nginx_status 주소   |
| listenPort   | 에이전트 포트 설정        |

### 3. Nginx agent 설치
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

