# Kubernetes 환경에서 DataSaker 설정파일 구성하기
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
  runtimeSock: ${VAR_RUNTIME_SOCK}
EOF
```

## 헬름을 이용한 DataSaker 설정파일 구성
### DataSaker 헬름 레포지토리 추가
```shell
helm repo add datasaker https://datasaker.github.io/agent-helm/
```
<!--
### 데이터세이커 헬름 가져오기
```shell
helm pull datasaker/agent-helm
```
### 압축 해제
```shell
tar -zxvf agent-helm-0.1.0.tgz -C ~/datasaker
```

### DataSaker 설치
```shell
helm install datasaker ~/datasaker/agent-helm -n datasaker --create-namespace \
  -f ~/datasaker/config.yaml
```
-->
### Helm install을 통하여 DataSaker 설정파일 생성
```shell
helm install datasaker datasaker/agent-helm -n datasaker --create-namespace \
  -f ~/datasaker/config.yaml
```
