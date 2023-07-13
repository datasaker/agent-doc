# DataSaker Documentation

## README.md 작성 가이드

> 작성 목적

- 이 파일은 설치와 관련된 README.md 파일을 작성하기 위한 가이드입니다.
- 문서 작성에 있어 통일성을 위해 작성하였습니다.
- 가급적이면 이 가이드를 따라 작성해주세요. 가이드에 맞지 않는 부분이 있을 경우 수정해주세요.

---

### 1. 문서 작성

#### 1.1. 마크다운 문서 작성 규칙

- 문서 작성 시, 문서의 제목은 `#`으로 작성합니다.
- 한글 문서의 경우, 각 고유명사를 한글로 작성합니다.
- 각 에이전트의 **고유 명사**에는 `` 를 사용하여 강조합니다.  (ex: `데이터세이커 에이전트` 설치방법)
- 헤더의 경우, 고유 명사를 강조하지 않습니다. (ex: # 데이터세이커 에이전트 설치방법)
- `#` ~ `#####` 헤더의 위 아래에는 빈 줄을 넣습니다.
- 텍스트 박스 안에 코드를 작성할 때는, ` ``` `를 사용합니다.
- 텍스트 박스 위 아래에는 빈 줄을 넣습니다.
- 불필요한 공백은 제거합니다. (ex: 문장 끝 공백, 문장 끝 줄바꿈)

#### 1.2. 한글 문서 작성시의 경어체 사용

- 문서 작성 시, 경어체를 사용합니다.
- 단순 지시문의 경우, 평어체를 사용합니다.
- 단순 지시문의 경우, `>` 혹은 숫자 배열을 사용하여 표시합니다. (quotation | ordered list 사용)

#### 1.3. 명사 표기

- **데이터세이커** : 데이터세이커의 고유명사로, 데이터세이커의 제품명을 의미합니다.
- **DataSaker** : 데이터세이커의 영문표기입니다. 중간 공백을 넣지 않습니다.
- 한글 문서의 경우, **데이터세이커**를 사용합니다. 마찬가지로, 도커, 쿠버네티스 등의 한글 표기를 사용합니다.
  - 예외사항 : MySQL, PostgreSQL 등의 명사는 영문으로 표기합니다.
- 각 에이전트의 명칭은 아래 표를 따릅니다.

| 에이전트 | 명칭 | 명칭(영문) | 설명 |
| --- | --- | --- | --- |
| agent | 에이전트 | Agent | 데이터세이커의 데이터 수집 에이전트를 일반적으로 일컫는 말 |
| - | 기본 에이전트 | Base Agent | 데이터세이커의 기본 에이전트; 각 인프라 환경에 설치되는 에이전트 설명할때 사용됩니다. (ex: dsk-container-agent + dsk-node-agent) |
| dsk-node-agent | 데이터세이커 노드 에이전트 | DataSaker Node Agent | 데이터세이커 노드 에이전트는 데이터세이커의 모니터링 대상이 되는 서버에 설치되어, 서버의 상태를 수집하여 데이터세이커 서버로 전송합니다. |
| dsk-elasticsearch-agent | 데이터세이커 엘라스틱서치 에이전트 | DataSaker Elasticsearch Agent | 데이터세이커 엘라스틱서치 에이전트는 데이터세이커의 모니터링 대상이 되는 엘라스틱서치의 상태를 수집하여 데이터세이커 서버로 전송합니다. |
| dsk-container-agent | 데이터세이커 컨테이너 에이전트 | DataSaker Container Agent | 데이터세이커 컨테이너 에이전트는 데이터세이커의 모니터링 대상이 되는 컨테이너에 설치되어, 컨테이너의 상태를 수집하여 데이터세이커 서버로 전송합니다. docker 환경에서 dsk-node-agent와 함께 배포되도록 가이드 됩니다. |
| dsk-kubernetes-agent | 데이터세이커 쿠버네티스 에이전트 | DataSaker Kubernetes Agent | 데이터세이커 쿠버네티스 에이전트는 데이터세이커의 모니터링 대상이 되는 쿠버네티스에 설치되어, 쿠버네티스의 상태를 수집하여 데이터세이커 서버로 전송합니다. dsk-kube-state-agent와 dsk-k8s-agent 컨테이너가 한 파드로 배포됩니다. |
| dsk-mongo-agent | 데이터세이커 몽고 에이전트 | DataSaker Mongo Agent | 데이터세이커 몽고 에이전트는 데이터세이커의 모니터링 대상이 되는 몽고DB에 설치되어, 몽고DB의 상태를 수집하여 데이터세이커 서버로 전송합니다. |
| dsk-mysql-agent | 데이터세이커 MySQL 에이전트 | DataSaker MySQL Agent | 데이터세이커 MySQL 에이전트는 데이터세이커의 모니터링 대상이 되는 MySQL에 설치되어, MySQL의 상태를 수집하여 데이터세이커 서버로 전송합니다. |
| dsk-postgres-agent | 데이터세이커 PostgreSQL 에이전트 | DataSaker PostgreSQL Agent | 데이터세이커 PostgreSQL 에이전트는 데이터세이커의 모니터링 대상이 되는 PostgreSQL에 설치되어, PostgreSQL의 상태를 수집하여 데이터세이커 서버로 전송합니다. |

##### 1.1.1 문서 작성 예시

```markdown
# 문서 제목

## 1. 문서 작성

### 1.1. 문서 작성 규칙

- 문서 작성 시, 문서의 제목은 `#`으로 작성합니다.
- 이 글은 **데이터세이커** 문서입니다.
- This document was written for **DataSaker**.

> 텍스트 박스 작성시 아래 규칙을 따른다.

~~~ yaml
텍스트 박스의 위 아래줄은 공백입니다.
~~~
```

---

## 문서 관리 규칙

### 1. 문서 관리

- 설치 문서는 install-guide 디렉터리에서 관리합니다.
- 설치 문서의 변경점이 배포되기 위해서, install-guide 아래의 한글 문서는 gitbook/ko 디렉터리로 이동되어야 합니다.
- 설치 문서에 대한 번역은 gitbook/ja, gitbook/en 디렉터리에서 관리합니다.

### 2. 문서 배포 관리

- 문서 배포는 gitbook으로 관리됩니다.
- gitbook은 다음 링크에서 확인 가능합니다.
  - [https://app.gitbook.com/o/pWBl8U3ZAGulEDGWoOMA/home](https://app.gitbook.com/o/pWBl8U3ZAGulEDGWoOMA/home)
- 문서의 배포를 위해서는 github의 main 브랜치까지 변경사항이 반영되어야 합니다.
- 반영된 문서는 다음 링크에서 확인 가능합니다.
  - [https://datasaker.gitbook.io/datasaker/](https://datasaker.gitbook.io/datasaker/)
