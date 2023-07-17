#  Kubernetes環境でDatasaker設定ファイルを構成する

`DataSaker`は、`Kubernetes`環境で`Helm`を介したインストールガイドに進みます。 （`ヘルム`がインストールされていない場合は、ヘルムのインストールを先に進んでください。https://helm.sh/ja/docs/intro/install/）

## ユーザー情報を登録する

`DataSaker`設定ファイルを構成するためのユーザーテナント情報を登録します。

### ディレクトリの作成

```shell
mkdir -p ~/datasaker
chmod 755 ~/datasaker
```

### ユーザー情報登録ファイルの生成

```shell
cat << EOF> ~/datasaker/config.yaml
userInfo:
  clusterName: ${VAR_CLUSTER_NAME}
  apiKey: ${VAR_GLOBAL_APIKEY}
  runtimeType: ${VAR_RUNTIME_TYPE}
EOF
```

各情報は以下を意味します。

|情報説明
| ----------- | ---------------------------------------- |
| clusterName |ユーザーが付けるキューバネティスクラスタ名。 |
| apiKey | DataSaker から発行された API Key。 |
| runtimeType |キューバネティスランタイムの種類。 （docker、containerd、crio）|

＃＃ヘルムを使用したDataSaker設定ファイルの設定

### DataSakerヘルムレポジトリの追加

```shell
helm repo add datasaker https://datasaker.github.io/agent-helm/
```

### Helm installによるDataSaker設定ファイルの生成

```shell
helm install datasaker datasaker/agent-helm -n datasaker --create-namespace \
  -f ~/datasaker/config.yaml
```

新しいチャートを追加する場合は、`helm repo update`を進めてください。