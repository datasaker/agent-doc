# dsk-kubernetes-agent

## Kubernetes環境でDataSaker Kubernetes agentをインストールする

`Kubernetes agent`は、クーバネティスクラスタの状態をリアルタイムで収集します。これにより、クラスター内のノードやパードの状態、リソース使用量、イベントなど、さまざまな情報を収集できます。また、Cubernetes APIサーバーと連動してクラスタノードやリソースの生成イベントをリアルタイムで検出し、問題発生時に適切な対応を行うことができます。収集したデータを分析して、クラスターのパフォーマンスの向上と安定性の向上に貢献できます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (README.md)

## Kubernetes agentのインストール

### 1. Kubernetes agent 設定値の登録
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
### 2. Kubernetes agentのインストール
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
