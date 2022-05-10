# Ansible For VPS
VPSの初期設定に関するAnsible Playbookです。

現在以下のPlaybookが用意されています。

- Ubuntu 20.04の初期セットアップ (`setup_vps.yml`)
  - 作業ユーザ作成
  - SSHポート変更
  - UFW設定
- セットアップ後のCPU Minerのインストール (`install_cpuminer`)
- セットアップ後のシングルノードKubernetesのインストール (`setup_kubernetes.yml`)

# Usage

## SSH鍵の準備
`~/.ssh/id_rsa.pub`に公開鍵を、`~/.ssh/id_rsa`に秘密鍵を配置してください。

## hostsの準備
以下を実行してhostsファイルを準備してください。

その際`{{}}`で囲まれた部分は置換してください。

```shell
cp hosts-template hosts
vim hosts
```

## private.ymlの準備
以下を実行してprivate.ymlファイルを準備してください。

その際`{{}}`で囲まれた部分は置換してください。

```shell
cp private-template.yml private.yml
vim private.yml
```

その後、必要があればAnsible Vaultで暗号化してください。

ただし、以下の手順は暗号化しない場合のものです。

## 実行
以下を実行してください。

`${USER}`をセットアップ時に作成した作業ユーザを指定して実行してください。

```shell
ansible-playbook -i hosts ${PLAYBOOK} --extra-vars="@private.yml" -u ${USER}
```

例

```shell
ansible-playbook -i hosts install_nodejs.yml --extra-vars="@private.yml" -u user
```

ただし`setup_vps.yml`の場合は以下で実行してください (rootに公開鍵認証でSSHできる場合は末尾の`--ask-pass`は不要です)。

```shell
ansible-playbook -i hosts setup_vps.yml --extra-vars="@private.yml" -u root --ask-pass
```