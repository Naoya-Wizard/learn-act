
# トリガーが push のテスト手順

この文書では、特定のブランチ（例えば `main` ブランチ）へのプッシュがトリガーとなる GitHub Actions ワークフローをローカルでテストする手順を説明します。

## 前提条件

- `act` がインストールされていること。インストールがまだの場合は、Windowsでは Chocolatey を通じて `act` をインストールしてください。
- Docker Desktop がインストールされ、実行されていること。
- GitHub リポジトリのローカルクローンが存在すること。

## ワークフローファイルの準備

1. GitHub リポジトリ内に `.github/workflows` ディレクトリを作成します（もしまだ存在しない場合）。
2. ディレクトリ内に `branch-specific-trigger.yml` という名前のワークフローファイルを作成し、以下の内容を記述します。

    ```yaml
    name: Branch Specific Workflow

    on:
      push:
        branches:
          - main

    jobs:
      build:

        runs-on: ubuntu-latest
        steps:
        - name: Checkout Code
          uses: actions/checkout@v2
        - name: Run a script
          run: echo "This workflow runs only on pushes to the main branch."
    ```

## ローカルでのテスト実行

以下のコマンドを使用して、ローカル環境で `main` ブランチへのプッシュをシミュレートし、ワークフローをテストします。

```bash
act push -W .github/workflows/branch-specific-trigger.yml -P ubuntu-latest=catthehacker/ubuntu:act-latest
```

- `-W` オプションはワークフローファイルを指定します。
- `-P` オプションは Docker イメージを指定します。この例では `catthehacker/ubuntu:act-latest` を使用しています。

## 結果の確認

コマンドの実行後、コンソール出力を確認し、次の点をチェックします。

- ワークフローが正しくトリガーされたか。
- 各ステップが成功しているか。
- 期待される出力（この例では "This workflow runs only on pushes to the main branch."）が表示されているか。

```bash
act push -W .github/workflows/branch-specific-trigger.yml -P ubuntu-latest=catthehacker/ubuntu:act-latest
time="2024-04-20T21:28:39+09:00" level=info msg="Using docker host 'npipe:////./pipe/docker_engine', and daemon socket 'npipe:////./pipe/docker_engine'"
[Branch Specific Workflow/build] 🚀  Start image=catthehacker/ubuntu:act-latest
[Branch Specific Workflow/build]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
[Branch Specific Workflow/build]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
[Branch Specific Workflow/build]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
[Branch Specific Workflow/build] ⭐ Run Main Checkout Code
[Branch Specific Workflow/build]   🐳  docker cp src=D:\naoyadirectory\Gitリポジトリ\TestActions\. dst=/mnt/d/naoyadirectory/Gitリポジトリ/TestActions
[Branch Specific Workflow/build]   ✅  Success - Main Checkout Code
[Branch Specific Workflow/build] ⭐ Run Main Run a script
[Branch Specific Workflow/build]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /var/run/act/workflow/1] user= workdir=
| This workflow runs only on pushes to the main branch.
[Branch Specific Workflow/build]   ✅  Success - Main Run a script
[Branch Specific Workflow/build] Cleaning up container for job build
[Branch Specific Workflow/build] 🏁  Job succeeded
```
