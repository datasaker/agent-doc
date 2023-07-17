# dsk-mongo-agent

## Kubernetes環境でDataSaker Mongo agentをインストールする

## DataSaker 先行作業を行いましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](dsk-mongo-agent/ja/$%7BPREPARATION\_MANUAL\_JP%7D/)

## Mongo agentのインストール

### 1. Mongo agent 設定値登録

```shell
cat << EOF >> ~/datasaker/config.yaml

mongoAgent:
  list: []
EOF
```

#### Mongo agent設定値

Mongo agentの設定値の意味とdefault値は次のとおりです。ユーザーごとにエージェント設定に異なる要件があります。したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。最適な結果を得るためにエージェント設定を調整してください。 `\~/datasaker/config.yaml`でその値を追加または変更します。

``` yaml
```

### 2. Mongo agentのインストール

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```