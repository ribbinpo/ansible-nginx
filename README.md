# Ansible Nginx Setup with Docker-Compose

## Overview

This project uses Ansible to automate the setup of an Nginx server with Docker Compose. It includes two playbooks to configure Nginx for HTTP and HTTPS with SSL.

## Prerequisites

- Python engine (can skip if you have ansible in your machine)
- At least 1 server (OS: Ubuntu)
- SSL certificate and private key (for HTTPS setup)

## Installation

1. create python enviroment

```sh
python3 -m venv env
```

2. Activate python enviroment

```sh
source env/bin/activate # for macOS
```

for deactivate `source deactivate`

3. install ansible package

```sh
pip3 install -r requirement.txt
```

4. install docker compose galaxy collection

```sh
ansible-galaxy collection install community.docker --force
```

## Usage

### For HTTP

1. Create `inventory` file and write server connection

```sh
cp inventory_example inventory
```

2. Go to `host_vars/server01` file and fill in:

```sh
cp host_vars/server01_example host_vars/server01
```

- PRIVATE_IP: private ip on your server
- PUBLIC_IP: public ip on your server
- PORT: server port that you want mapping

3. Run the http playbook

```sh
ansible-playbook playbooks/nginx-http.yaml
```

### For HTTPS

1. Create `inventory` file and write server connection

```ini
[all:vars]
ansible_user=

[all]
server01 ansible_host=
ansible_password=
```

2. Go to `host_vars/server01` file and fill in:

- PRIVATE_IP: private ip on your server
- PUBLIC_IP: public ip on your server
- PORT: server port that you want mapping
- DOMAIN_NAME: your domain name that map to this server

#### [1] Have own SSL

3. Move certificate and private key file to `files/ssl` dicectory

- change certificate filename to `certificate.crt`
- change privatekey filename to `private.key`

4. Run the https playbook

```sh
ansible-playbook playbooks/nginx-https.yaml
```

#### [2] (Alternative) Generate ssl by Certbot with public registry

3. Run the https playbook

```sh
ansible-playbook playbooks/nginx-https-auto-ssl.yaml
```

#### [3] (Alternative) Generate ssl by Certbot with private registry

```sh
ansible-playbook playbooks/nginx-https-auto-ssl-private.yaml
```

## Check syntax

```sh
ansible-playbook --syntax-check playbooks/nginx-https.yaml
```
