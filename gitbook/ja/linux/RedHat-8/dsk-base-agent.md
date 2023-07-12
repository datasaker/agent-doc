#dsk-base-agent

`Base agent`はサーバから発生する様々な情報をリアルタイムで収集します。
たとえば、メモリ、CPU使用率など、サーバーのパフォーマンス指標、ネットワークトラフィック、コンテナ情報など、さまざまな情報を収集できます。
これにより、顧客はリアルタイムでサーバーの状態を監視でき、サーバーのパフォーマンスを最適化し、信頼性を向上させることができます。
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

＃Datasaker先行作業を行いましたか？
現在RedHat 8環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作]（$ {PREPARATION_MANUAL_JP}）

# Base agentのインストール
## 1. パッケージのインストール
`DataSaker`の `Base agent`をインストールするにはsudo権限が必要です。
「シェル
sudo yum install dsk-node-agent
「」

## 2. Base agentの設定
「シェル
sudo vi /etc/datasaker/dsk-node-agent/agent-config.yml
「」
必要に応じて次の内容を修正します。
`` yaml
# Base agent 設定ファイル
agent:
  agent_name: "dsk-base-agent" # エージェント名 (エイリアス) default=dsk-node-agent
  cluster_id: "my-cluster-id" # 管理対象となる環境がどのクラスタにまとめられているかに関する設定 default=unknown
「」

## 3. パッケージの実行
「シェル
sudo systemctl enable dsk-node-agent --now
「」

## 4. パッケージ実行状態の確認
「シェル
sudo systemctl status dsk-node-agent
「」

# Base agentを削除する
## 1. パッケージの中断
「シェル
sudo systemctl stop dsk-node-agent
「」

## 2. パッケージの削除
「シェル
sudo systemctl stop dsk-node-agent &&
sudo yum remove dsk-node-agent
「」