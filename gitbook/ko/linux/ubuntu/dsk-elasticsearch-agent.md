# Ubuntu 환경에 DataSaker Elasticsearch agent 설치하기 (Beta)
`Elasticsearch agent`는 elasticsearch 상태 정보를 수집합니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Ubuntu 환경에서는 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${PREPARATION_MANUAL_KR})

# Elasticsearch agent 설치하기
## 1. 패키지 설치
`DataSaker`의 `Elasticsearch agent`를 설치하기 위해서는 sudo 권한이 필요합니다.
<!-- 
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-elasticsearch-agent
```

## 2. Elasticsearch agent 설정값 등록

```shell
vi /etc/datasaker/dsk-elasticsearch-agent/agent-config.yml
```

필요에 따라 다음 내용을 수정합니다.

```yaml
# Elasticsearch agent 설정 파일
agent:
  agent_name: "dsk-elasticsearch-agent" # 에이전트 이름 (별칭) default=dsk-elasticsearch-agent
```

## 3. 패키지 실행

```bash
systemctl start dsk-elasticsearch-agent
```

## 4. 패키지 실행 상태 확인

```bash
systemctl status dsk-elasticsearch-agent
```

또는

```bash
service dsk-elasticsearch-agent status
```

# Arguments

elasticsearch exporter의 argument 설정을 변경하여 사용자 환경에 맞게 수정할 수 있습니다. 설정 값은 다음과 같습니다.

| Argument                | Description                                                                                                                                                                                                                                                                                                                                                                      | Default               |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| es.uri                  | Address (host and port) of the Elasticsearch node we should connect to. This could be a local node (localhost:9200, for instance), or the address of a remote Elasticsearch server. When basic auth is needed, specify as: <proto>://<user>:<password>@<host>:<port>. E.G., http://admin:pass@localhost:9200. Special characters in the user credentials need to be URL-encoded. | http://localhost:9200 |
| es.all                  | If true, query stats for all nodes in the cluster, rather than just the node we connect to.                                                                                                                                                                                                                                                                                      | false                 |
| es.cluster_settings     | If true, query stats for cluster settings.                                                                                                                                                                                                                                                                                                                                       | false                 |
| es.indices              | If true, query stats for all indices in the cluster.                                                                                                                                                                                                                                                                                                                             | false                 |
| es.indices_settings     | If true, query settings stats for all indices in the cluster.                                                                                                                                                                                                                                                                                                                    | false                 |
| es.indices_mappings     | If true, query stats for mappings of all indices of the cluster.                                                                                                                                                                                                                                                                                                                 | false                 |
| es.aliases              | If true, include informational aliases metrics.                                                                                                                                                                                                                                                                                                                                  | true                  |
| es.shards               | If true, query stats for all indices in the cluster, including shard-level stats (implies es.indices=true).                                                                                                                                                                                                                                                                      | false                 |
| es.snapshots            | If true, query stats for the cluster snapshots.                                                                                                                                                                                                                                                                                                                                  | false                 |
| es.slm                  | If true, query stats for SLM.                                                                                                                                                                                                                                                                                                                                                    | false                 |
| es.data_stream          | If true, query state for Data Steams.                                                                                                                                                                                                                                                                                                                                            | false                 |
| es.timeout              | Timeout for trying to get stats from Elasticsearch. (ex: 20s)                                                                                                                                                                                                                                                                                                                    | 5s                    |
| es.ca                   | Path to PEM file that contains trusted Certificate Authorities for the Elasticsearch connection.                                                                                                                                                                                                                                                                                 |                       |
| es.client-private-key   | Path to PEM file that contains the private key for client auth when connecting to Elasticsearch.                                                                                                                                                                                                                                                                                 |                       |
| es.client-cert          | Path to PEM file that contains the corresponding cert for the private key to connect to Elasticsearch.                                                                                                                                                                                                                                                                           |                       |
| es.clusterinfo.interval | Cluster info update interval for the cluster label                                                                                                                                                                                                                                                                                                                               | 5m                    |
| es.ssl-skip-verify      | Skip SSL verification when connecting to Elasticsearch.                                                                                                                                                                                                                                                                                                                          | false                 |
| aws.region              | Region for AWS elasticsearch                                                                                                                                                                                                                                                                                                                                                     |                       |

---
# Elasticsearch agent 제거하기
## 1. 패키지 중단
```bash
systemctl stop dsk-elasticsearch-agent
```
또는
```bash
service dsk-elasticsearch-agent stop
```

## 2. 패키지 제거
```bash
sudo apt remove dsk-elasticsearch-agent
```

