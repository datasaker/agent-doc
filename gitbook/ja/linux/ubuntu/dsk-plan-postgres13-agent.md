# dsk-plan-postgres13-agent

`Postgres13 agent`は、データベースの状態とスロークエリをリアルタイムで収集します。
これにより、データベースのパフォーマンス指標、リソース使用量、スロークエリなど、さまざまな情報を収集できます。
収集されたデータに基づいて、データベースのパフォーマンスのボトルネックを特定し、対応できます。
さらに、スロークエリを検出して、インデックスの作成、クエリの最適化などの方法でデータベースのパフォーマンスを向上させることができます。
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

＃DataSaker先行作業を行いましたか？
現在のUbuntu環境では、`DataSaker`の先行作業が進行しなかった場合は、`DataSaker`先行作業を先に進めてください。 [DataSaker先行操作]（$ {PREPARATION_MANUAL_JP}）

# Plan Postgres13 Agentのインストール
## 1. agent-configの設定

``` yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres13-agent" # replace you want
  data_source_name:
    user: # <user_name>
    password: # <user_password>
    address: # <database_address>
    port: # <database_port>
    DBName: # <database_name>
  explain:
    scrape_interval: 30s # <activity_session_scrape_time>
    scrape_timeout: 5s # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s # <slow_query_standard>
    executor_number: 10 # <explain executor number>
    sender_number: 10 # <explain sender number>
    activity_query_buffer: 50 # <activity query buffer>
    plan_sender_buffer: 50 # <explain result buffer>
```

## 2. パッケージのインストール
sudo 権限が必要です。
`` bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres13-agent
```

## 3. パッケージの実行
`` bash
sudo dsk-plan-postgres13-agent start
```

## 4. パッケージ実行状態の確認
### Running
`` bash
sudo dsk-plan-postgres13-agent status
```
### Not Running
`` bash
sudo dsk-plan-postgres13-agent status
```

# Plan Postgres13 Agent を削除する
## 1. パッケージの中断
`` bash
sudo dsk-plan-postgres13-agent stop
```

## 2. パッケージの削除
`` bash
sudo apt remove dsk-plan-postgres13-agent
```