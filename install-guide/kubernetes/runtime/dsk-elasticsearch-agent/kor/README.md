# 쿠버네티스 환경에서 DataSaker Elasticsearch agent 설치하기

`Elasticsearch agent`는 DataSaker에서 elasitcsearch 정보를 수집하는 agent입니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Elasticsearch agent 설치하기
## 1. Elasticsearch agent 설정값 등록
elasticsearchAgent.list[].uri에 elasticsearch addresss정보를 반드시 등록해주세요
```shell
cat << EOF >> ~/datasaker/config.yaml

elasticsearchAgent:
  list:
    - name: 'es-1'
      uri: 'https://es_id:es_pw@elasticsearch_service.namespace.svc.cluster.local:9200'
      tolerations: []
      imgPolicy: 'Always'
      imgVersion: 'latest'
      logLevel: 'INFO'
      listenPort: 19114
      exporterArgs:
        - --es.timeout=5s
      extraArgs: []
      extraEnvs:
        - name: ENV_1
          value: ENV_1
      resources:
        requests:
          cpu: 100m
          memory: 512Mi
        limits:
          cpu: 1000m
          memory: 1000Mi
EOF
```

각 설정 값에 대한 자세한 설명은 다음과 같습니다.

| 설정           | 설명                |
|--------------|-------------------|
| name         | agent의 이름         |
| uri          | elasticsearch 주소  |
| tolerations  | 쿠버네티스 톨러레이션 설정    |
| imgPolicy    | 쿠버네티스 이미지 정책 설정   |
| imgVersion   | 에이전트 이미지 버전 설정    |
| logLevel     | 에이전트 로그 레벨 설정     |
| listenPort   | 에이전트 포트 설정        |
| exporterArgs | 에이전트 옵션 설정        |
| extraArgs    | 에이전트 추가 옵션 설정     |
| extraEnvs    | 에이전트 추가 환경변수 설정   |
| resources    | 에이전트 쿠버네티스 리소스 설정 |

## 2. Elasticsearch agent 설치
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

더 자세한 정보가 필요하시면 다음 문서를 참고해주세요. [관련 문서](../../../../../settings/dsk-elasticsearch-agent/settings.md)
