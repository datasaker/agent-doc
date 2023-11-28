# dsk-cloudwatch-agent

`Cloudwatch agent`는 고객이 AWS에서 사용하는 Cloud Watch 서비스와 연계하여 고객의 AWS 서비스를 모니터링하는 agent 입니다.

## Datasaker 선행 작업을 진행하였나요?

현재 환경에서 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## CloudWatch agent 설치하기

### 패키지 설치

```shell
yum install dsk-cloudwatch-agent
```

### 1. CloudWatch agent 설정

CloudWatch agent를 연동하기 위하여 AWS에 관련된 인증 정보를 설정해야 합니다.

다음 파일을 수정하여 Access 정보를 등록합니다.

#### `/etc/datasaker/dsk-cloudwatch-agent/agent.env`

```shell
AWS_ACCESS_KEY_ID=<YOUR_AWS_ACCESS_KEY_ID>
AWS_SECRET_ACCESS_KEY=<YOUR_AWS_SECRET_KEY>
```

### 2. 패키지 실행

```shell
systemctl enable dsk-cloudwatch-agent --now
```

### 3. 패키지 실행 상태 확인

```shell
systemctl status dsk-cloudwatch-agent
```

## CloudWatch agent 제거하기

### 1. 패키지 중단

```shell
systemctl stop dsk-cloudwatch-agent
```

### 2. 패키지 제거

```shell
yum remove dsk-cloudwatch-agent
```
