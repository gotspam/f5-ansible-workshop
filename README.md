# <img align="center" src="img/ansible.svg" width="70">&nbsp;&nbsp; f5-ansible
[![Build Status](https://img.shields.io/travis/ArtiomL/f5-ansible/develop.svg)](https://travis-ci.org/ArtiomL/f5-ansible)
[![Releases](https://img.shields.io/github/release/ArtiomL/f5-ansible.svg)](https://github.com/ArtiomL/f5-ansible/releases)
[![Commits](https://img.shields.io/github/commits-since/ArtiomL/f5-ansible/latest.svg?label=commits%20since)](https://github.com/ArtiomL/f5-ansible/commits/master)
[![Maintenance](https://img.shields.io/maintenance/yes/2018.svg)](https://github.com/ArtiomL/f5-ansible/graphs/code-frequency)
[![Issues](https://img.shields.io/github/issues/ArtiomL/f5-ansible.svg)](https://github.com/ArtiomL/f5-ansible/issues)
[![Docker Hub](https://img.shields.io/docker/pulls/artioml/f5-ansible.svg)](https://hub.docker.com/r/artioml/f5-ansible/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](/LICENSE)
[![Slack Status](https://f5cloudsolutions.herokuapp.com/badge.svg)](https://f5cloudsolutions.herokuapp.com)

&nbsp;&nbsp;

## Table of Contents
- [Description](#description)
- [Installation](#installation)
	- [Run](#run)
	- [Credentials](#credentials)
- [Playbooks](#playbooks)
	- [Deploy](#deploy)
	- [Teardown](#teardown)
	- [Service Template](#service-template)
	- [Infrastructure as Code](#infrastructure-as-code)
- [Demos](#demos)
- [Help](#--help)
- [License](LICENSE)

&nbsp;&nbsp;

## Description

Essential Ansible container(s) with F5 modules, extensible playbooks and Slack notifications.

&nbsp;&nbsp;

## Installation

### Run
```shell
# Stable
docker run -it artioml/f5-ansible

# Stable, runs as root (for Drone CI/CD)
docker run -it artioml/f5-ansible:su

# Experimental
docker run -it artioml/f5-ansible:dev
```

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
```
