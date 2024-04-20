
# `act` のインストールガイド（Windows）

このガイドでは、Windows 環境で `act` を Chocolatey を使用してインストールする方法について説明します。`act` はローカルで GitHub Actions ワークフローを実行するツールです。

## 前提条件

- 推奨は Windows 10 以上
- PowerShell 5 (またはそれ以上) または CMD
- インターネット接続

## 手順

### 1. Chocolatey のインストール

Chocolatey がまだインストールされていない場合は、以下の手順に従ってインストールしてください。

1. **管理者権限で PowerShell を開く**:
   - スタートメニューを右クリックし、「Windows PowerShell (管理者)」を選択します。

2. **Chocolatey インストールスクリプトの実行**:
   - 以下のコマンドを PowerShell に貼り付けて実行します。

   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```

   - このスクリプトは Chocolatey をダウンロードし、インストールします。

### 2. `act` のインストール

Chocolatey を使用して `act` をインストールするには、以下の手順に従います。

1. **再び管理者権限で PowerShell を開く**:
   - 前述の方法で PowerShell (管理者) を開きます。

2. **`act` のインストールコマンドを実行**:
   - 以下のコマンドを PowerShell に入力し、実行します。

   ```powershell
   choco install act-cli
   ```

   - このコマンドは Chocolatey を通じて `act` をインストールします。

### 3. `act` の動作確認

インストールが完了したら、次のコマンドを実行して `act` が正しくインストールされているか確認します。

```powershell
act --version
```

このコマンドが `act` のバージョンを表示すれば、インストールは成功しています。
