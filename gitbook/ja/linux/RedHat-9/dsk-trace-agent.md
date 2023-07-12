#dsk-trace-agent

`Trace agent`はopentelemetryやJaegerのようなオープンソースの分散追跡システムと連携して、アプリケーションの分散追跡データを収集します。
これにより、アプリケーション内のさまざまなサービス間の通信を追跡し、パフォーマンスのボトルネックを特定して最適化できます。
収集されたデータは迅速に処理され、リアルタイムで監視および分析が可能です。
お客様のニーズに合わせて「Trace Agent」設定を調整して、最適な結果を提供します。

＃Datasaker先行作業を行いましたか？
現在RedHat 9環境で `DataSaker`の先行操作が進行していない場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作]（$ {PREPARATION_MANUAL_JP}）

# Trace agentのインストール
## 1. パッケージのインストール
「シェル
sudo yum install dsk-trace-agent
「」

## 2. Trace agentの設定
「シェル
sudo vi /etc/datasaker/dsk-trace-agent/agent-config.yml
「」
必要に応じて次の内容を修正します。

`` yaml
# Trace agent 設定ファイル
agent:
  metadata:
    agent_name: "dsk-trace-agent" # エージェント名 (エイリアス) default=dsk-trace-agent
「」

## 3. パッケージの実行
「シェル
sudo systemctl enable dsk-trace-agent --now
「」

## 4. パッケージ実行状態の確認
「シェル
sudo systemctl status dsk-trace-agent
「」

# Trace agent ポート情報
|ポート| Protocol | Describe |
|-------|----------|----------------|
| 6831 | UDP | thrift-compact |
| 6832 | UDP | thrift-binary |
| 14250 | TCP | jaeger-grpc |
| 14268 | TCP | jaeger-http |
| 4317 | TCP | otlp-grpc |
| 4318 | TCP | otlp-http |

＃アプリケーションにトレースエージェントを連携する
Trace agentを利用するには、まずSDKでアプリケーションをインストルメントする必要があります。
詳しくはリンクをご覧ください。
[関連ドキュメントリンク]（https://github.com/datasaker/documentation/tree/main/settings/dsk-trace-agent/Instrumentation）

# Trace agentの設定
必要に応じて、以下のドキュメントを参照してトレースエージェントの設定を変更できます。

## Trace agent 設定値
Trace agentの設定値の意味するデフォルト値は次のとおりです。
ユーザーごとにエージェント設定に異なる要件があります。
したがって、エージェント設定をユーザー設定に合わせて調整する必要があります。
最適な結果を得るためにエージェント設定を調整してください。
エージェント設定ファイルのパスは `/etc/datasaker/dsk-trace-agent/agent-config.yml` です。
設定ファイルに値を追加または変更します。

ファイルの構造は以下の通りです。

### `agent-config.yml`
`` yaml
agent:
  ＃エージェントのメタデータ
  metadata: <metadata>
  ＃エージェントの実行関連オプション
  option:
    [ collector_config: <collector_config> ]
        [ reciever_config: <reciever_config> ]
「」

#### `metadata`
`` yaml
# エージェント名(エイリアス)
[ agent_name: <string> | default = "dsk-trace-agent" ]

＃管理対象となる環境がどのクラスタにまとめられているかについての設定
[cluster_id：<cluster_id> | default = "unknown" ]
「」

#### `collector_config`
`` yaml
#collectorに適用されるサンプリング率
＃100以上の場合、すべてのデータが収集されます
[sampling_rate：<float> | default = 1 ]
「」

#### `receiver_config`
`` yaml
＃collectorからデータを受け取るポート番号
[ listen_port: <uint16> | | default = 14251 ]

＃各スパンに適用されるカスタムタック
[ custom_tags: <map[string]string> | default = "" ]
「」

### Example
`` yaml
agent:
  metadata:
    agent_name: dsk-trace-agent # エージェント名 (エイリアス) default=dsk-trace-agent
  option:
    collector_configs:
      sampling_rate: 1
「」

# Trace agentを削除する
## 1. パッケージの中断
「シェル
sudo systemctl stop dsk-trace-agent
「」

## 2. パッケージの削除
「シェル
sudo systemctl stop dsk-trace-agent &&
sudo yum remove dsk-trace-agent
「」