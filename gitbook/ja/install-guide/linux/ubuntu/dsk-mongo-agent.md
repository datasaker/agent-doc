# dsk-mongo-agent

## DataSaker先行作業を進めましたか？

現在のUbuntu環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker先行操作](README.md)

## Mongo agentのインストール

### 1. パッケージのインストール

`DataSaker`の`Mongo agent`をインストールするにはsudo権限が必要です。
<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-mongo-agent
```
### 2. Mongo agentの設定
```shell
vi /etc/datasaker/dsk-mongo-agent/agent-config.yml
```
必要に応じて次の内容を修正します。
```yaml
# Base agent 설정 파일
agent:
  agent_name: "dsk-mongo-agent" # 에이전트 이름 (별칭) default=dsk-mongo-agent
```
### 3. パッケージの実行
```bash
systemctl start dsk-mongo-agent
```
### 4. パッケージ実行状態の確認
```bash
systemctl status dsk-mongo-agent
```
または
```bash
service dsk-mongo-agent status
```
## パッケージの設定方法

MongoDBを監視するためにエージェント設定を変更する必要があるかもしれません。 \

### Args引数リスト

`DataSaker` Mongo agentの設定を追加できます。
次の文書を参照してください。 [関連文書](../../../../../settings/dsk-mongo-agent/settings.md)

---

## Mongo agentを削除する

### 1. パッケージの中断
```bash
systemctl stop dsk-mongo-agent
```
または
```bash
service dsk-mongo-agent stop
```
### 2. パッケージの削除
```bash
sudo apt remove dsk-mongo-agent
```
