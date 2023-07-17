# dsk-trace-agent

`Trace agent`はopentelemetryやJaegerのようなオープンソースの分散追跡システムと連携して、アプリケーションの分散追跡データを収集します。
これにより、アプリケーション内のさまざまなサービス間の通信を追跡し、パフォーマンスのボトルネックを特定して最適化できます。
収集されたデータは迅速に処理され、リアルタイムで監視および分析が可能です。
お客様のニーズに合わせて`Trace Agent`設定を調整して、最適な結果を提供します。

＃DataSaker先行作業を行いましたか？

現在のUbuntu環境では、`DataSaker`の先行操作が進行していない場合は、`DataSaker`先行操作を先に進めてください。
願います。 [DataSaker先行操作]（$ {PREPARATION_MANUAL_JP}）

# Trace agentのインストール

## 1. パッケージのインストール

<!--
example API Key : VAR_GLOBAL_APIKEY=1234567890abcdef1234567890abcdef
 -->

```shell
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-trace-agent
```

## 2. Trace agentの設定

```shell
vi /etc/datasaker/dsk-trace-agent/agent-config.yaml
```

必要に応じて次の内容を修正します。

``` yaml
# Trace agent 設定ファイル
agent:
  agent_name: "your_agent_name_what_you_want" # default=trace-agent
```

## 3. パッケージの実行

```shell
systemctl enable dsk-trace-agent --now
```

## 4. パッケージ実行状態の確認

```shell
sudo systemctl status dsk-trace-agent
```

# Trace agent ポート情報

|ポート| Protocol | Describe |
|-------|----------|----------------|
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

# ApplicationにTrace Agentを連携する

Trace agentを利用するには、まずSDKでアプリケーションを計測する必要があります。詳しくはリンクをご覧ください。
[関連ドキュメントリンク]（https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation）

# Trace agentの設定

必要に応じて、以下のドキュメントを参照してトレースエージェントの設定を変更できます。

## Trace agent 設定値

Trace agentの設定値の意味とdefault値は次のとおりです。ユーザーごとにエージェント設定に異なる要件があります。したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。最適な結果を得るために
エージェント設定を調整してください。エージェント設定ファイルのパスは `/etc/datasaker/dsk-trace-agent/agent-config.yaml` です。
設定ファイルに値を追加または変更します。

ファイルの構造は以下の通りです。

### `agent-config.yml`

``` yaml
agent:
  ＃エージェントのメタデータ
  metadata: <metadata>
  ＃エージェントの実行関連オプション
  option:
    [ collector_config: <collector_config> ]
    [ reciever_config: <reciever_config> ]
```

#### `metadata`

``` yaml
# エージェント名(エイリアス)
[ agent_name: <string> | default = "dsk-trace-agent" ]

＃管理対象となる環境がどのクラスタにまとめられているかについての設定
[cluster_id：<cluster_id> | default = "unknown" ]
```

#### `collector_config`

``` yaml
#collectorに適用されるサンプリング率
＃100以上の場合、すべてのデータが収集されます
[sampling_rate：<float> | default = 1 ]
```

#### `reciever_config`

``` yaml
＃collectorからデータを受け取るポート番号
[ listen_port: <uint16> | | default = 14251 ]

# 各spanに適用されるカスタムタグ
[ custom_tags: <map[string]string> | default = "" ]
```

### Example
``` yaml
agent:
  metadata:
    agent_name: dsk-trace-agent # エージェント名 (エイリアス) default=dsk-trace-agent
  option:
    collector_config:
      sampling_rate: 1 # span データ sampling 比率 (0~100) default=1
```

# Trace agentを削除する

## 1. パッケージの中断

```shell
systemctl stop dsk-trace-agent
```

## 2. パッケージの削除

```shell
sudo apt remove dsk-trace-agent
```