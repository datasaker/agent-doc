# dsk-plan-mysql-agent

`Mysql agent`は、データベースの状態とスロークエリをリアルタイムで収集します。これにより、データベースのパフォーマンス指標、リソース使用量、スロークエリなど、さまざまな情報を収集できます。収集されたデータに基づいて、データベースのパフォーマンスのボトルネックを特定し、対応できます。さらに、スロークエリを検出して、インデックスの作成、クエリの最適化などの方法でデータベースのパフォーマンスを向上させることができます。お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## Supported version

| version |サポート|
| ------------ | ------- |
| MySQL 8.0.33 | O |

## DataSaker先行作業を進めましたか？

現在のUbuntu環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker 先行操作]($%7BPREPARATION\_MANUAL\_KR%7D/)

## Plan MySQL Agent install

### 1. MySQL設定の変更

管理したいデータベース `performance_schema=ON` モジュールがアクティブであることを確認してください。
[performance\_schemaリファレンスサイト]（https://dev.mysql.com/doc/refman/8.0/en/performance-schema-quick-start.html）

### 2. MySQL User 権限の設定

`MySQL agent`をインストールするには、 `MySQL User`に権限を付与する必要があります。\
`MySQL user`の権限を確認し、権限がない場合は権限を付与してください。
必要なユーザー権限は次のとおりです。

* `SELECT`
* `UPDATE`
* `DELETE`
* `INSERT`

[MySQL user権限参照サイト](https://dev.mysql.com/doc/refman/8.0/en/grant.html)

### 3. パッケージのインストール
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-plan-mysql-agent
```
### 4. MySQL agent 設定値の登録

#### 必須入力項目

必須入力項目は次のとおりです。 MySQLの設定に合わせて値を入れてください

|エンティティ説明|
| --------------------------------- | ----------------------- |
| agent.data\_source\_name.user | MySQL User IDを入力します。 |
| agent.data\_source\_name.password | MySQL User パスワードを入力します。 |
| agent.data\_source\_name.address | MySQLアドレスを入力してください。 |
| agent.data\_source\_name.port | MySQLポートを入力します。 |
| agent.data\_source\_name.DBName | MySQLデータベース名を入力します。 |

#### オプション入力
```shell
vi /etc/datasaker/dsk-plan-mysql-agent/agent-config.yml
```
必要に応じて次の内容を修正します。
```yaml
agent:
  metadata:
    agent_name: "dsk-plan-mysql-agent"
  data_source_name:
    user: 'user'
    password: 'pass'
    address: '127.0.0.1'
    port: '3306'
    DBName: 'database'
  explain:
    scrape_interval: 30s
    scrape_timeout: 5s
    slow_query_standard: 5s
    executor_number: 10
    sender_number: 10
    activity_query_buffer: 50
    plan_sender_buffer: 50
```
### 5. パッケージの実行
```bash
sudo dsk-plan-mysql-agent start
```
### 6. パッケージ実行状態の確認

#### Running
```bash
systemctl status dsk-plan-mysql-agent
```
または
```shell
serivce dsk-plan-mysql-agent
```
## Log agentを削除する

### 1. パッケージの中断
```bash
sudo dsk-plan-mysql-agent stop
```
### 2. パッケージの削除
```bash
sudo apt remove dsk-plan-mysql-agent
```
