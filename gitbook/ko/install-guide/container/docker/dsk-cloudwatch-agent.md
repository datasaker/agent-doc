# dsk-cloudwatch-agent

## 도커 환경에 DataSaker CloudWatch agent 설치 하기

`Cloudwatch agent`는 고객이 AWS에서 사용하는 Cloud Watch 서비스와 연계하여 고객의 AWS 서비스를 모니터링하는 agent 입니다.


### 지원 서비스 목록

현재 지원중인 AWS 서비스는 다음과 같습니다.

| AWS 서비스                 | 설명                               |
|-------------------------|----------------------------------|
| `AWS/ECS`               | Elastic Container Service 기본 메트릭 |
| `ECS/ContainerInsights` | ECS Container Insights 관련 메트릭    |
| `AWS/RDS`               | RDS 관련 메트릭                       |

## DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

## Cloudwatch agent 설치하기

### 설정하기

#### 1-1. 환경 변수 설정

Cluster ID를 설정하면 자원을 논리적인 그룹으로 관리할 수 있습니다. Cluster ID를 설정하지 않으면 기본값으로 `unknown`이 설정됩니다.

Cluster ID를 설정하기 위해선 다음과 같이 환경 변수를 설정해야 합니다.

```shell
export DSK_CLUSTER_ID=my-cluster
export AWS_ACCESS_KEY_ID=<YOUR_AWS_ACCESS_KEY_ID>
export AWS_SECRET_ACCESS_KEY=<YOUR_AWS_SECRET_ACCESS_KEY>
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

### 설치하기

1. 데이터세이커가 사용할 로컬 디렉터리를 생성합니다.

```shell
   sudo mkdir -p /var/datasaker
   sudo chown -R datasaker:datasaker /var/datasaker/
```

2. 도커 명령어를 서버에 입력합니다.

```shell
docker run -d --name dsk-cloudawtch-agent\
   -v /var/datasaker/:/var/datasaker/\
   -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
   -e DSK_CLUSTER_ID=${DSK_CLUSTER_ID} \
   -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
   -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
   datasaker/dsk-cloudwatch-agent

sudo chown -R datasaker:datasaker /var/datasaker/
```

### 제한 사항

AWS 자원에 별도의 Tag가 없는 경우, Cloudwatch agent는 해당 자원을 모니터링 하지 않습니다.

Cloudwatch Agent를 사용함으로 추가로 발생되는 비용에 대해서는 아래 페이지를 참고해주세요.

- [Amazon CloudWatch 요금](https://aws.amazon.com/ko/cloudwatch/pricing/)
