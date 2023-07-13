＃Docker環境でDataSaker設定ファイルを構成する

## ユーザー情報を登録する

`DataSaker`設定ファイルを構成するための環境を構成し、ユーザーテナント情報を登録します。

サーバーの端末を開き、次のコマンドを入力します。

```shell
sudo groupadd -g 202306 datasaker
sudo useradd -r -u 202306 -g datasaker -s /usr/sbin/nologin datasaker

mkdir -p ~/.datasaker
chmod 755 ~/.datasaker

cat << EOF> ~/.datasaker/config.yml
global:
  api_key: ${VAR_GLOBAL_APIKEY}
  gates:
    trace_datagate:
      url:gate.jp.datasaker.io:31300
    manifest_datagate：
      url:gate.jp.datasaker.io:31301
    metric_datagate:
      url:gate.jp.datasaker.io:31302
    plan_datagate:
      url:gate.jp.datasaker.io:31303
    loggate:
      url:gate.jp.datasaker.io:31304
  agent_manager:
    url: api.jp.datasaker.io
EOF
```