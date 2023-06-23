# JAVA

Java 애플리케이션은 수동, 혹은 자동으로 instrumentation 되어질 수 있습니다. 이 가이드에서는 **opentelemetry-javaagent**를 사용하여 자동 instrumentaion을 수행하는 방법을 설명합니다.

## 요구 사항

* **Java 8 이상**

## Java agent 다운로드

javaagent는 [github 페이지](https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest)에서 다운로드할 수 있습니다.
다음 명령어로 최신 버전을 다운로드 받을 수 있습니다.

```bash
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar
```

## 설정하기

각 실행 환경에 맞게 설정을 진행 합니다.

### Non-Container 환경

어플리케이션을 실행할 때 다음과 같이 시스템 프로퍼티를 추가합니다.

``` bash
java -javaagent:path/to/opentelemetry-javaagent.jar \
     -Dotel.exporter.otlp.traces.endpoint=localhost:4317 \
     -Dotel.service.name=your-service-name \
     -Dotel.resource.attributes=dsk.host.key=$(cat /var/datasaker/host_key) \
     -Dotel.metrics.exporter=none \
     -Dotel.logs.exporter=none \
     -jar myapp.jar
```

### Docker 환경

별도로 네트워크를 구성하여 데이터를 전송하거나, 호스트 포트를 사용하여 데이터를 전송할 수 있습니다.

Docker 네트워크 설정을 통해 Trace Agent로 접근할 수 있도록 설정 합니다.

``` bash
    docker network create <network-name>
```

Trace Agent를 다음과 같이 배포합니다.

``` bash
     docker run -d --name dsk-trace-agent\
       -v /var/datasaker/:/var/datasaker/\
       -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
       -e DKS_LOG_LEVEL=info\
       -p 4317:4317/tcp\
       -p 4318:4318/tcp\
       --restart=always\
       --network <network-name>
       datasaker/dsk-trace-agent
```

도커 이미지에 다음과 같은 환경 변수들을 추가하여 배포합니다.

``` bash
    # 자바 어플리케이션 실행 예시
    docker run my-java-application \
        -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=localhost:4317 \
        -e OTEL_SERVICE_NAME=your-service-name \
        -e OTEL_METRICS_EXPORTER=none \
        -e OTEL_LOGS_EXPORTER=none \
        -e OTEL_RESOURCE_ATTRIBUTES=dsk_host_key=$(cat /var/datasaker/host_key) \
        --network <network-name> \
        my/docker-image
        -d
```

두번째 방법은, 호스트 포트를 사용하는 방법입니다. 다음과 같이 호스트 주소를 사용하여를 추가합니다.

Trace Agent를 다음과 같이 배포합니다.

``` bash
     docker run -d --name dsk-trace-agent\
       -v /var/datasaker/:/var/datasaker/\
       -v ~/.datasaker/config.yml:/etc/datasaker/global-config.yml:ro\
       -e DKS_LOG_LEVEL=info\
       -p 4317:4317/tcp\
       -p 4318:4318/tcp\
       --restart=always\
       datasaker/dsk-trace-agent
```
도커 이미지에 다음과 같은 환경 변수들을 추가하여 배포합니다.
``` bash
    docker run my-java-application \
        -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=localhost:4317 \
        -e OTEL_SERVICE_NAME=your-service-name \
        -e OTEL_METRICS_EXPORTER=none \
        -e OTEL_LOGS_EXPORTER=none \
        -e OTEL_RESOURCE_ATTRIBUTES=dsk_host_key=$(cat /var/datasaker/host_key) \
        my/docker-image
        -d
```

### Kubernetes 환경

Dockerfile 생성 시 다음과 같이 opentelemetry javaagent를 추가합니다.

``` dockerfile
    # Dockerfile 예시
    FROM openjdk:8-jre-alpine
    COPY opentelemetry-javaagent.jar /app/
    COPY myapp.jar /app/
    WORKDIR /app
    CMD ["java", "-javaagent:opentelemetry-javaagent.jar", "-jar", "myapp.jar"]
```

배포 되는 yaml 파일에 아래와 같이 환경변수를 설정 합니다.

``` yaml
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        env:
        # 아래와 같이 환경 변수들을 추가합니다.
        - name: DSK_NODE_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: spec.nodeName
        - name: DSK_NAMESPACE
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
        - name: DSK_POD_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.name
        - name: OTEL_SERVICE_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.labels['app.kubernetes.io/name'] # 혹은 서비스를 구분할 수 있는 다른 label
        - name: OTEL_EXPORTER_OTLP_ENDPOINT
          value: http://dsk-trace-agent.dsk-agent-dev:4317 # dsk trcace agent 서비스명
        - name: OTEL_METRICS_EXPORTER
          value: none
        - name: OTEL_LOGS_EXPORTER
          value: none
        - name: OTEL_RESOURCE_ATTRIBUTES
          value: dsk_node_name=$(DSK_NODE_NAME),dsk_namespace=$(DSK_NAMESPACE),dsk_pod_name=$(DSK_POD_NAME),dsk_app_name=$(DSK_NAMESPACE)_$(DSK_POD_NAME)
```
