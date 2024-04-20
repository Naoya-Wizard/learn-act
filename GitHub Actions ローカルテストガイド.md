# GitHub Actions ローカルテストガイド

このガイドでは、`act` を使用して GitHub Actions ワークフローをローカル環境でテストする方法を詳しく説明します。

## 前提条件

- Docker Desktop がインストールされていること（Windowsの場合、Linuxコンテナモードであることを確認）
- `act` がインストールされていること

## 手順

### 1. Docker と `act` のインストール

Docker と `act` のインストール方法は以下の通りです。

#### Docker のインストール

1. [Dockerの公式サイト](https://www.docker.com/products/docker-desktop)から Docker Desktop をダウンロードし、インストールします。
2. Docker が正しくインストールされていることを確認するために、次のコマンドを実行します。
   ```bash
   docker run hello-world
   ```

#### act のインストール
act は Chocolatey（Windows）、Homebrew（macOS）、または直接ダウンロード（Linux）を通じてインストールできます。

### 2. GitHub リポジトリの準備
ローカルに GitHub リポジトリをクローンします。

```bash
git clone https://github.com/your-username/your-repository.git
cd your-repository
```

### 3. ワークフローの定義
.github/workflows ディレクトリにワークフローファイル（.yml）を作成します。

```yml
name: Example Workflow
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Run a script
      run: echo "Hello, world!"
```

### 4. ワークフローのローカル実行
act コマンドを使用してワークフローをローカルで実行します。

```bash
act push -P ubuntu-latest=catthehacker/ubuntu:act-latest
```

このコマンドは、ubuntu-latest 環境に catthehacker/ubuntu:act-latest イメージを使用することを指定します。

## 結果の確認
act の実行が完了した後、コンソールに表示される出力を確認し、すべてのステップが正常に完了したかどうかを確認します。

```bash
act push -P ubuntu-latest=catthehacker/ubuntu:act-latest
time="2024-04-20T20:22:59+09:00" level=info msg="Using docker host 'npipe:////./pipe/docker_engine', and daemon socket 'npipe:////./pipe/docker_engine'"
[Hello World Workflow/greet] 🚀  Start image=catthehacker/ubuntu:act-latest
[Hello World Workflow/greet]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
[Hello World Workflow/greet]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
[Hello World Workflow/greet]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
[Hello World Workflow/greet] ⭐ Run Main Checkout code
[Hello World Workflow/greet]   🐳  docker cp src=D:\naoyadirectory\Gitリポジトリ\TestActions\. dst=/mnt/d/naoyadirectory/Gitリポジトリ/TestActions
[Hello World Workflow/greet]   ✅  Success - Main Checkout code
[Hello World Workflow/greet] ⭐ Run Main Run a one-line script
[Hello World Workflow/greet]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /var/run/act/workflow/1] user= workdir=
| Hello, World!
[Hello World Workflow/greet]   ✅  Success - Main Run a one-line script
[Hello World Workflow/greet] Cleaning up container for job greet
[Hello World Workflow/greet] 🏁  Job succeeded
```
解説

- Docker環境設定: Dockerホストとデーモンのソケットを使用します。
- イメージの取得: catthehacker/ubuntu:act-latest イメージをプルします。
- コンテナの作成と実行: tail -f /dev/null をエントリポイントとして、コンテナを作成し実行します。
- ソースコードのチェックアウト: ローカルディレクトリからコンテナ内にコードをコピーします。
- スクリプトの実行: Bash を使用してコンテナ内でスクリプトを実行し、"Hello, World!" を出力します。
- クリーンアップ: ジョブ完了後、使用したコンテナをクリーンアップします。

