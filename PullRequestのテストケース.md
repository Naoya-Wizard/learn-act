
### トリガーが Pull Request のテスト手順

このワークフローは、プルリクエストの作成や更新時にトリガーされます。この設定は、プルリクエストが`main`ブランチへマージされる前に自動テストやビルドを行うのに特に適しています。

#### ワークフローファイルの作成

1. GitHubリポジトリ内の`.github/workflows`ディレクトリを用意します（存在しない場合は作成します）。
2. 次に、以下の内容を含むYAMLファイルを作成します。ファイル名は`pull-request-workflow.yml`などとします。

```yaml
name: Pull Request Workflow

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v2
    - name: Run Tests
      run: echo "Running tests..."
    - name: Build
      run: echo "Building the application..."
```

このファイルは、`main` ブランチへのプルリクエスト時に実行されるジョブを定義しており、シンプルなテストとビルドのステップを含んでいます。実際のプロジェクトでは、ここに必要なビルドコマンドやテストスクリプトを配置します。

#### ローカルでのテスト

`act` を使ってこのワークフローをローカルでテストするには、次のコマンドを使用します。このコマンドは、`main`ブランチへのプルリクエストイベントをシミュレートして、対応するワークフローを実行します。

```bash
act pull_request -W .github/workflows/pull-request-workflow.yml -P ubuntu-latest=catthehacker/ubuntu:act-latest
```

- `-W` オプションは特定のワークフローファイルを指定するために使用されます。
- `-P` オプションは使用するDockerイメージを指定します。

`act` はこのYAMLファイルを解釈し、指定された環境でワークフローをシミュレートして実行結果を表示します。これにより、実際にGitHubにプルリクエストを作成する前にローカル環境で動作を確認できます。

## 結果の確認

コマンドの実行後、コンソール出力を確認し、次の点をチェックします。

- ワークフローが正しくトリガーされたか。
- 各ステップが成功しているか。
- 期待される出力（この例では "This workflow runs only on pushes to the main branch."）が表示されているか。

```bash
act pull_request -W .github/workflows/pull-request-workflow.yml -P ubuntu-latest=catthehacker/ubuntu:act-latest
time="2024-04-20T22:28:31+09:00" level=info msg="Using docker host 'npipe:////./pipe/docker_engine', and daemon socket 'npipe:////./pipe/docker_engine'"
[Pull Request Workflow/test] 🚀  Start image=catthehacker/ubuntu:act-latest
[Pull Request Workflow/test]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
[Pull Request Workflow/test]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
[Pull Request Workflow/test]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
[Pull Request Workflow/test] ⭐ Run Main Checkout Code
[Pull Request Workflow/test]   🐳  docker cp src=D:\naoyadirectory\Gitリポジトリ\TestActions\. dst=/mnt/d/naoyadirectory/Gitリポジトリ/TestActions
[Pull Request Workflow/test]   ✅  Success - Main Checkout Code
[Pull Request Workflow/test] ⭐ Run Main Run Tests
[Pull Request Workflow/test]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /var/run/act/workflow/1] user= workdir=
| Running tests...
[Pull Request Workflow/test]   ✅  Success - Main Run Tests
[Pull Request Workflow/test] ⭐ Run Main Build
[Pull Request Workflow/test]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /var/run/act/workflow/2] user= workdir=
| Building the application...
[Pull Request Workflow/test]   ✅  Success - Main Build
[Pull Request Workflow/test] Cleaning up container for job test
[Pull Request Workflow/test] 🏁  Job succeeded
```
