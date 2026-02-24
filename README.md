# SessionAttested PoC Repo
このリポジトリは、[SessionAttested](https://github.com/shizuku198411/SessionAttested)を利用して開発を行うPoCリポジトリです。

## 開発の流れ
### Workspace登録(初期化)
作業ディレクトリで `attested workspace init`を実行し、対話形式でWorkspace設定を行います。
```bash
$ attested workspace init

# 任意のWorkspaceID
workspace name (workspace-id): attested_poc
# Workspaceパス,CWDが自動でセットされる
workspace path (workspace-host) [/path/to/attested_poc]:
# Dev Containerのイメージ、デフォルトでSSH接続が行えるubuntuイメージがビルドされます
docker image [attested_base:latest]:
is the image need build or pull? [pull|build] (default: build): 
# Dev Container内でgit pushを行う場合に、ホスト側のssh keyを利用するかどうか
mount host git ssh key into container? [y/N]: y
host ssh private key path [/path/to/.ssh/github_id_key]: 
# GitHubのリポジトリ/ユーザ情報
GitHub repo (owner/name) [optional]: shizuku198411/SessionAttested-PoC-Repo    
git user.name [optional]: shizuku198411
git user.email [optional]: <EMAIL>

workspace_id: attested_poc
workspace_meta: /path/to/attested_poc/.attest_run/state/workspaces/attested_poc.json
workspace_host: /path/to/attested_poc
container_id: fe975f3474d71698a219d0c4c424d5a2b6321c0288216830b5271895ff86ac8e
container_name: attested-poc-dev
created: true
state: container is created/registered and left stopped
scaffold:
  created: .gitignore (managed block)
  created: attest/attested.yaml
  created: attest/Dockerfile
  created: attest/policy.yaml
```
これにより、

- このリポジトリ内で監査を行うための設定ファイル/ディレクトリ群
- Dev Containerの作成

が行われます。

※Dev Containerの実体は `docker container ls -a` で確認できます。
```bash
$ docker container ls -a
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS                    PORTS     NAMES
fe975f3474d7   attested_base:latest   "/usr/sbin/sshd -D -e"   4 minutes ago   Created                             attested-poc-dev
```

### Session開始
Dev Containerの起動および監査を開始するため、`attested start` を実行します。

```bash
$ attested start
session_id: 2db0423d7d36c88c7e03e61d5d3c7234
session_dir: .attest_run/state/sessions/2db0423d7d36c88c7e03e61d5d3c7234
workspace_host: /path/to/attested_poc
container_id: fe975f3474d71698a219d0c4c424d5a2b6321c0288216830b5271895ff86ac8e
image: attested_base:latest
container_reused: true
meta: .attest_run/state/sessions/2db0423d7d36c88c7e03e61d5d3c7234/meta.json
collector_auto: true
collector_pid: .attest_run/state/sessions/2db0423d7d36c88c7e03e61d5d3c7234/collector.pid
collector_log: .attest_run/state/sessions/2db0423d7d36c88c7e03e61d5d3c7234/collector.log
generated_signing_key: .attest_run/keys/attestation_priv.pem
generated_public_key: .attest_run/keys/attestation_pub.pem
```

これで、監査ログ収集(attested collector)とDev Containerが起動します。

### Dev Container内作業
ユーザによって利用しやすい環境/接続方式でDev Container内で作業を行います。

- `docker container exec` による作業
- ターミナルからのSSH接続
- IDEからのSSH接続

など、任意のものを利用できます。SessionAttestedの監査アーキテクチャはユーザの開発手段/ツールを制限しません。

