# dsk-kubernetes-agent

## 쿠버네티스 환경에서 DataSaker Kubernetes agent 설치하기

`Kubernetes agent`는 쿠버네티스 클러스터의 상태를 실시간으로 수집합니다. 이를 통해 클러스터의 노드 및 파드의 상태, 리소스 사용량, 이벤트 등 다양한 정보를 수집할 수 있습니다. 또한, 쿠버네티스 API 서버와 연동하여 클러스터 노드 및 리소스의 생성 이벤트를 실시간으로 감지하고, 문제 발생 시 적절한 대응을 할 수 있습니다. 수집된 데이터를 분석하여 클러스터의 성능 개선 및 안정성 강화에 기여할 수 있습니다. 고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

## DataSaker 선행 작업을 진행하였나요?

현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](dsk-kubernetes-agent/kor/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Kubernetes agent 설치하기

### 1. Kubernetes agent 설정 값 등록

```shell
cat << EOF >> ~/datasaker/config.yaml

kubernetesAgent:
  enabled: true
  kubeStateAgent:
    logLevel: 'INFO'
  k8sAgent:
    logLevel: 'INFO'
EOF
```

### 2. Kubernetes agent 설치

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
