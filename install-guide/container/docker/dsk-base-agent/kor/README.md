# 도커 환경에 DataSaker Base agent 설치 하기

`Base agent`는 서버에서 발생하는 다양한 정보를 실시간으로 수집합니다.
예를 들어, 메모리, CPU 사용량 등 서버의 성능 지표, 네트워크 트래픽, Container 정보 등 다양한 정보를 수집할 수 있습니다.
이를 통해 고객은 서버의 상태를 실시간으로 모니터링할 수 있으며, 이를 기반으로 서버의 성능을 최적화하고 안정성을 높일 수 있습니다.
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?

현재 Docker 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Base agent 설치하기

Cluster ID를 설정하면 자원을 논리적인 그룹으로 관리할 수 있습니다. Cluster ID를 설정하지 않으면 기본값으로 `unknown`이 설정됩니다.

Cluster ID를 설정하기 위해선 다음과 같이 환경 변수를 설정해야 합니다.

```shell
export DSK_CLUSTER_ID=my-cluster
```

1. 데이터세이커가 사용할 로컬 디렉터리를 생성합니다.

```shell
   sudo mkdir -p /var/datasaker
   sudo chown -R datasaker:datasaker /var/datasaker/ 
```

2. 도커 명령어를 서버에 입력합니다.

```shell
docker run -d --name dsk-node-agent\
   -v /var/datasaker/:/var/datasaker/\
   -v /proc/:/host/proc/:ro\
   -v /sys/:/host/sys/:ro\
   -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
   -e DSK_CLUSTER_ID=${DSK_CLUSTER_ID} \
   -e DSK_LOG_LEVEL=DEBUG\
   --privileged\
   --restart=always\
   --network host\
   --pid host\
   datasaker/dsk-node-agent
docker run -d --name dsk-container-agent\
   -v /var/datasaker/:/var/datasaker/\
   -v /:/rootfs/:ro\
   -v /var/run/:/var/run/:ro\
   -v /sys/:/sys/:ro\
   -v /dev/disk/:/dev/disk/:ro\
   -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
   -e DSK_CLUSTER_ID=${DSK_CLUSTER_ID} \
   -e GOMAXPROCS=1\
   -e DSK_LOG_LEVEL=DEBUG\
   --privileged\
   --restart=always\
   datasaker/dsk-container-agent

sudo chown -R datasaker:datasaker /var/datasaker/ 
```
