# ansible-firewall-demo


quay.io/centos/centos:stream9
```
dnf update -y
dnf install -y glibc-all-langpacks git sudo which tree jq
dnf install -y epel-release && dnf install -y sshpass
dnf install -y python3 python3-pip
dnf install -y tmux
dnf clean all

pip install -U pip setuptools
pip install ansible-core
pip install boto boto3 awscli yq
pip install awxkit
pip install pypi pan-python pan-os-python

ansible-galaxy collection install community.aws
ansible-galaxy collection install community.general
ansible-galaxy collection install fortinet.fortios
ansible-galaxy collection install awx.awx
ansible-galaxy collection install paloaltonetworks.panos
ansible-galaxy collection install ansible.posix
```

ネットワーク環境の作成
```
ansible-playbook -i inventory_demo_init 01_demo_env_create_network.yaml
```

Firewall、サーバーのの起動（事前にイメージの購読が必要）
```
ansible-playbook -i inventory_demo_init 02_demo_env_create_server.yaml
```

- FortiGate の初回ログイン&パスワード変更を実施（モジュールからはできない？）
- PaloAlto のGUIパスワード設定を実施（コマンドラインから）

```
ssh -i ansible-firewall-demo-key.pem admin@xx.xx.xx.xx

configure
set mgt-config users admin password
commit
exit
exit
```

初期設定を投入
```
ansible-playbook -i inventory_demo_fortigate 11_demo_env_fortifate_init.yaml
ansible-playbook -i inventory_demo_clients 12_demo_env_server_http.yaml
ansible-playbook -i inventory_demo_palo 13_demo_env_panos_zone.yaml
```

AAP側の設定

別途準備して設定する


