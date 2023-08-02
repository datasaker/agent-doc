# Ubuntu 환경에 DataSaker Mongo agent 설치하기 (Beta)

# DataSaker 선행 작업을 진행하였나요?
현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](README.md)

# Mongo agent 설치하기
## 1. 패키지 설치
`DataSaker`의 `Mongo agent`를 설치하기 위해서는 sudo 권한이 필요합니다.
<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mongo-agent
```

## 2. Mongo agent 설정

```shell
vi /etc/datasaker/dsk-mongo-agent/agent-config.yml
```

필요에 따라 다음 내용을 수정합니다.

```yaml
# Base agent 설정 파일
agent:
  agent_name: "dsk-mongo-agent" # 에이전트 이름 (별칭) default=dsk-mongo-agent
```

## 3. 패키지 실행

```bash
systemctl start dsk-mongo-agent
```

## 4. 패키지 실행 상태 확인

```bash
systemctl status dsk-mongo-agent
```

또는

```bash
service dsk-mongo-agent status
```

### 패키지 설정 방법

MongoDB를 모니터링 하기 위해서 agent 설정 변경해야 할 수 있습니다. \

### Args 인자 목록

`DataSaker` Mongo agent의 설정을 추가 할 수 있습니다.
다음 문서를 참고해주세요. [관련 문서](../../../../../settings/dsk-mongo-agent/settings.md)

---
# Mongo agent 제거하기

## 1. 패키지 중단

```bash
systemctl stop dsk-mongo-agent
```

또는

```bash
service dsk-mongo-agent stop
```

## 2. 패키지 제거

```bash
sudo apt remove dsk-mongo-agent
```
