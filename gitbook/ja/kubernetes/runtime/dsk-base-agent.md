#dsk-base-agent

## Kubernetes環境でDataSaker Base agentをインストールする

`Base agent`はサーバから発生する様々な情報をリアルタイムで収集します。たとえば、メモリ、CPU使用率など、サーバーのパフォーマンス指標、ネットワークトラフィック、コンテナ情報など、さまざまな情報を収集できます。これにより、顧客はリアルタイムでサーバーの状態を監視でき、サーバーのパフォーマンスを最適化し、信頼性を向上させることができます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## DataSaker 先行作業を行いましたか？

現在Kubernetes環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker 先行操作] (dsk-base-agent/ja/$%7BPREPARATION\_MANUAL\_KR%7D/)

## Base agentのインストール

### 1. Base agent 設定値の登録

「シェル
cat << EOF >> ~/datasaker/config.yaml

baseAgent：
  enabled: true
  enableMaster: true
  nodeAgent:
    logLevel: 'INFO'
  containerAgent:
    logLevel: 'INFO'
EOF
「」

各設定値の詳細な説明は以下の通りである。

|設定|説明
| ----------------------------------- | -------------------------------------------------- |
| `baseAgent.enabled` | `base agent`を有効にするかどうかを決定します。 |
| `baseAgent.enableMaster` | `base agent`を`control-plane`ノードにもデプロイするかどうかを決定します。 |
| `baseAgent.nodeAgent.logLevel` | `node agent`のログレベルを設定します。 |
| `baseAgent.containerAgent.logLevel` | `container agent`のログレベルを設定します。 |

### 2. Base agentのインストール

「シェル
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
「」

##トラブルシューティング

### ポート使用問題

Base Agentは監視のために、インストール環境で2つのポートを使用します。 （default = `14194`、`19110`）そのポートをサーバー上の他のプログラムがすでに占有している場合、Base agentは正常に起動できず、次のログを出力します。

「シェル
ts=2023-03-14T02:54:49.460Z caller=node_exporter.go:201 level=error err="listen tcp :19110: bind: address already in use"
{"level":"error","ts":"2023-03-14T02:54:49Z","msg":"error occurred from subprocess","error":"error occured during running child process. err: exit status 1"}
{"level":"error","ts":"2023-03-14T02:54:49Z","msg":"error occurred from subprocess","error":"exporter exit unexpectedly and retry count exceeded"}
{"level":"fatal","ts":"2023-03-14T02:54:49Z","msg":"child process is terminated unexpectedly, please check other logs","error":"exporter exit unexpectedly and retry count exceeded"}
「」

この問題を解決するために、config.yamlファイルに `listenPort`設定を追加できます。

たとえば、 `base agent`内の `node agent`が使用する19110ポートを19111に置き換えるために、次のように設定を変更できます。

`` yaml
baseAgent：
  enabled: true
  enableMaster: true
  nodeAgent:
    logLevel: 'INFO'
    listenPort: 19111
  containerAgent:
    logLevel: 'INFO'
「」