# dsk-rabbitmq-agent
## 쿠버네티스 환경에서 DataSaker RabbitMQ agent 설치하기
`RabbitMQ agent`는 DataSaker에서 RabbitMQ 정보를 수집하는 agent입니다.

## DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## RabbitMQ agent 설치하기
### 1. RabbitMQ agent 설정값 등록
rabbitmqAgent.list[]에 rabbitmq 정보를 반드시 등록해주세요
```shell
cat << EOF >> ~/datasaker/config.yaml

rabbitmqAgent:
  list:
    - name: 'rabbitmq-1'
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      resources: {}
      tolerations: []
      extraArgs: []
      extraEnvs: []
      rabbitmqAddr: "http://localhost:15672"
      rabbitmqUser: "root"
      rabbitmqPass: "root"
      rabbitmqCA: ""
      rabbitmqCert: ""
      rabbitmqKey: ""
      listenPort: 19419
EOF
```

각 설정 값에 대한 자세한 설명은 다음과 같습니다.

| 설정           | 설명                |
|--------------|-------------------|
| name         | agent의 이름         |
| imgPolicy    | 쿠버네티스 이미지 정책 설정   |
| imgVersion   | 에이전트 이미지 버전 설정    |
| logLevel     | 에이전트 로그 레벨 설정     |
| resources    | 에이전트 쿠버네티스 리소스 설정 |
| tolerations  | 쿠버네티스 톨러레이션 설정    |
| extraArgs    | 에이전트 추가 옵션 설정     |
| extraEnvs    | 에이전트 추가 환경변수 설정   |
| rabbitmqAddr | rabbitmq 주소        |
| rabbitmqUser | rabbitmq 사용자      |
| rabbitmqPass | rabbitmq 비밀번호     |
| rabbitmqCA   | rabbitmq CA 인증서    |
| rabbitmqCert | rabbitmq 인증서       |
| rabbitmqKey  | rabbitmq 인증서 키     |
| listenPort   | 에이전트 포트 설정        |

### 2. RabbitMQ agent 설치
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

