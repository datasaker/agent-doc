# Kubernetes 환경에 Datasaker 설정파일 구성하기

`DataSaker`는 `Kubernetes` 환경에서 `헬름`을 통한 설치 가이드를 진행합니다. (`헬름`이 설치되어 있지 않으시다면 헬름 설치를 먼저 진행하여 주시기 바랍니다. https://helm.sh/ko/docs/intro/install/)

## 사용자 정보 등록하기

`DataSaker` 설정파일 구성하기 위한 사용자 테넌트 정보를 등록합니다.

### 디렉토리 생성

```shell
mkdir -p ~/datasaker
chmod 755 ~/datasaker
```

### 사용자 정보 등록파일 생성

```shell
cat << EOF > ~/datasaker/config.yaml
userInfo:
  clusterName: ${VAR_CLUSTER_NAME}
  apiKey: ${VAR_GLOBAL_APIKEY}
  runtimeType: ${VAR_RUNTIME_TYPE}
EOF
```

각 정보는 다음을 의미합니다.

| 정보          | 설명                                       |
| ----------- | ---------------------------------------- |
| clusterName | 유저가 붙이는 쿠버네티스 클러스터 이름.                   |
| apiKey      | DataSaker에서 발급받은 API Key.                |
| runtimeType | 쿠버네티스 런타임 종류. (docker, containerd, crio) |

## 헬름을 이용한 DataSaker 설정파일 구성

### DataSaker 헬름 레포지토리 추가

```shell
helm repo add datasaker https://datasaker.github.io/agent-helm/
```

### Helm install을 통하여 DataSaker 설정파일 생성

```shell
helm install datasaker datasaker/agent-helm -n datasaker --create-namespace \
  -f ~/datasaker/config.yaml
```

새로운 차트를 추가하실 경우 `helm repo update`를 진행해 주세요.
