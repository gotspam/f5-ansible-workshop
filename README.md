
## Table of Contents
- [Description](#description)
- [Credentials](#credentials)
- [Playbooks](#playbooks)
	- [Deploy](#deploy)
	- [Teardown](#teardown)
	- [Service Template](#service-template)
- [Help](#--help)
- [License](LICENSE)

&nbsp;&nbsp;

## Description

The purpose of this repository is to provide basic demo of deploying BIG-IP
services using Ansible.

Credit given to https://github.com/ArtiomL/f5-ansible which includes Ansible container and full infrastructure-as-code demo.

&nbsp;&nbsp;

### Credentials
The encrypted [vault](https://docs.ansible.com/ansible/latest/vault.html) file ([creds.yaml](creds.yaml)) contains the BIG-IP credentials.

The vault password is: **_password_**

View:
```shell
ansible-vault view creds.yml
Vault password: password
bigip_user: "admin"
bigip_pass: "admin"
```

Modify:
```shell
ansible-vault edit creds.yml
Vault password: password
```

&nbsp;&nbsp;

## Playbooks

### Deploy
```shell
./runsible.py {playbook_name}
```
For example:
```shell
./runsible.py app
# Which executes:
# ansible-playbook playbooks/app.yaml -e @creds.yaml --ask-vault-pass
```

### Service Template
```shell
./runsible.py {playbook_name} -n [service_name] -i [service_ip] -g [service_group]
```
For example:
```shell
./runsible.py iapp -n iapp_Web1 -i 10.1.10.11 -g appservers
# Which executes:
# ansible-playbook playbooks/iapp.yaml -e @creds.yaml --ask-vault-pass -e service_name="iapp_Web1" -e service_ip="10.1.10.11" -e service_group="appservers"

./runsible.py iapp -n iapp_Web2 -i 10.1.10.12 -g webservers
# Which executes:
# ansible-playbook playbooks/iapp.yaml -e @creds.yaml --ask-vault-pass -e service_name="iapp_Web2" -e service_ip="10.1.10.12" -e service_group="webservers"
```

### List App Services
```shell
./runsible.py {playbook_name} -n [service_name] -i [service_ip]
```
For example:
```shell
./runsible.py lvs -n lvs -i 10.1.10.11
# Which executes:
# ansible-playbook playbooks/lvs.yaml -e @creds.yaml --ask-vault-pass -e service_ip="10.1.10.11"
```

### Toggle Pool Member Status
```shell
ansible-playbook playbooks/offline.yaml -e @creds.yaml --ask-vault-pass -e pool="app2_pl" -e pmhost="10.1.20.16" -e pmport="80"
ansible-playbook playbooks/online.yaml -e @creds.yaml --ask-vault-pass -e pool="app2_pl" -e pmhost="10.1.20.16" -e pmport="80"
```

### Teardown
```shell
./runsible.py -t {playbook_name}
```
For example:
```shell
./runsible.py -t app
# Which executes:
# ansible-playbook playbooks/app.yaml -e @creds.yaml --ask-vault-pass -e state="absent"

./runsible.py -t iapp -n iapp_Web1 -i 10.1.10.11 -g appservers
# Which executes:
# ansible-playbook playbooks/iapp.yaml -e @creds.yaml --ask-vault-pass -e service_name="iapp_Web1" -e service_ip="10.1.10.11" -e service_group="appservers" -e state="absent"
```
