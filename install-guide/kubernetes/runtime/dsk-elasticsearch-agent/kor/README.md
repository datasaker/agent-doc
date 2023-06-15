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

## 2. Elasticsearch agent 설치
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

더 자세한 정보가 필요하시면 다음 문서를 참고해주세요. [관련 문서](../../../../../settings/dsk-elasticsearch-agent/settings.md)
