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

ただし`setup_vps.yml`の場合は`${USER}`を`root`で、それ以外の場合はセットアップ時に作成した作業ユーザを指定して実行してください。

```shell
ansible-playbook -i hosts ${PLAYBOOK} --extra-vars="@private.yml" -u ${USER}
```