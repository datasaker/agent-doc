# dsk-cloudwatch-agent

## 쿠버네티스 환경에서 DataSaker CloudWatch agent 설치하기

`Cloudwatch agent`는 고객이 AWS에서 사용하는 Cloud Watch 서비스와 연계하여 고객의 AWS 서비스를 모니터링하는 agent 입니다.

### 지원 서비스 목록

현재 지원중인 AWS 서비스는 다음과 같습니다.

| AWS 서비스                 | 설명                               |
|-------------------------|----------------------------------|
| `AWS/ECS`               | Elastic Container Service 기본 메트릭 |
| `ECS/ContainerInsights` | ECS Container Insights 관련 메트릭    |
| `AWS/RDS`               | RDS 관련 메트릭                       |

## DataSaker 선행 작업을 진행하였나요?

현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Cloudwatch agent 설치하기

### 1. CloudWatch agent 설정 값 등록

#### 1-1. Access 정보 등록

```shell
cloudwatchAgent:
  list:
    - name: 'cw-1'
      accessKey: $YOUR_AWS_ACCES_KEY
      secretKey: $YOUR_AWS_SECRET_KEY
```

#### 1-2. 권한 설정

해당 계정에 대하여 다음 권한이 모든 리소에 대해 반드시 부여되어 있어야 합니다.

```Shell
"tag:GetResources"
"cloudwatch:GetMetricData"
"cloudwatch:GetMetricStatistics"
"cloudwatch:ListMetrics"
```

예를 들어, 다음 권한으로 에이전트에 필요한 권한을 설정할 수 있습니다.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "tag:GetResources",
                "cloudwatch:GetMetricData",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:ListMetrics"
            ],
            "Resource": "*"
        }
    ]
}
```

### 2. CloudWatch agent 설치

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

### 제한 사항

AWS 자원에 별도의 Tag가 없는 경우, Cloudwatch agent는 해당 자원을 모니터링 하지 않습니다.

Cloudwatch Agent를 사용함으로 추가로 발생되는 비용에 대해서는 아래 페이지를 참고해주세요.

- [Amazon CloudWatch 요금](https://aws.amazon.com/ko/cloudwatch/pricing/)
