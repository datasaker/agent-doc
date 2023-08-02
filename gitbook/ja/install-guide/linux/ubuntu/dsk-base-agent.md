# dsk-base-agent

`base-agent` はサーバから発生する様々な情報をリアルタイムで収集します。
たとえば、メモリ、CPU使用率など、サーバーのパフォーマンス指標、ネットワークトラフィック、コンテナ情報など、さまざまな情報を収集できます。
これにより、お客様はリアルタイムでサーバーの状態を監視でき、サーバーのパフォーマンスを最適化し、信頼性を向上させることができます。
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## Datasaker先行作業を行いましたか？

現在の環境で `DataSaker` の先行操作が進まない場合は、`DataSaker` 先行操作を先に進めてください。 [DataSaker先行操作]（$ {PREPARATION_MANUAL_JP}）

## Base agentのインストール

### 1. パッケージのインストール

`DataSaker`の`base-agent`をインストールするにはsudo権限が必要です。
<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-node-agent
```
## 2. Base agentの設定
```shell
vi /etc/datasaker/dsk-node-agent/agent-config.yml
```
必要に応じて次の内容を修正します。
```yaml
agent:
  agent_name: "dsk-base-agent"
  cluster_id: "my-cluster-id"
```
各設定の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent_name` |エージェント名（エイリアス）| dsk-base-agent |いいえ
| `cluster_id` |管理対象となる環境がどのクラスタにまとめられているかを設定します。 | unknown |いいえ

### 3. パッケージの実行
```shell
systemctl enable dsk-node-agent --now
```
### 4. パッケージ実行状態の確認
```shell
systemctl status dsk-node-agent
```
## Base agentを削除する

### 1. パッケージの中断
```shell
systemctl stop dsk-node-agent
```
### 2. パッケージの削除
```bash
sudo apt remove dsk-node-agent
```
