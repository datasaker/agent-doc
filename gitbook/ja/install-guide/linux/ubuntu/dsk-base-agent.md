# dsk-base-agent

`Base agent`はサーバから発生する様々な情報をリアルタイムで収集します。
たとえば、メモリ、CPU使用率など、サーバーのパフォーマンス指標、ネットワークトラフィック、コンテナ情報など、さまざまな情報を収集できます。
これにより、顧客はリアルタイムでサーバーの状態を監視でき、サーバーのパフォーマンスを最適化し、信頼性を向上させることができます。
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

＃DataSaker先行作業を行いましたか？
現在のUbuntu環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker先行操作]（$ {PREPARATION_MANUAL_JP}）

# Base agentのインストール
## 1. パッケージのインストール
`DataSaker`の `Base agent`をインストールするにはsudo権限が必要です。
<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
``` bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-node-agent
```

## 2. Base agentの設定

```shell
vi /etc/datasaker/dsk-node-agent/agent-config.yml
```

必要に応じて次の内容を修正します。

``` yaml
# Base agent 設定ファイル
agent:
  agent_name: "dsk-base-agent" # エージェント名 (エイリアス) default=dsk-node-agent
  cluster_id: "my-cluster-id" # 管理対象となる環境がどのクラスタにまとめられているかについての設定 default=unknown
```

## 3. パッケージの実行
``` bash
systemctl start dsk-node-agent
```

## 4. パッケージ実行状態の確認
``` bash
systemctl status dsk-node-agent
```
または
``` bash
service dsk-node-agent status
```

---
# Base agentを削除する
## 1. パッケージの中断
``` bash
systemctl stop dsk-node-agent
```

## 2. パッケージの削除
``` bash
sudo apt remove dsk-node-agent
```