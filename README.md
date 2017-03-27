# gemfire-ansible

GemFire automation through Ansible

## Requirements

* Pip (installed in setup script if missing)
* Ansible (installed in setup script if missing)

## Optional (Mac Only)

* Homebrew (installed in setup script if missing)

## Local Testing (Mac Only)

This playbook is meant to be self-contained as much as possible. The bootstrap script will install minimal required softwares, e.g. pip, ansible, and brew to the Mac machine.

Download the folowing files and place under gemfire-ansible/files/:
* jdk-8u121-linux-x64.rpm (from Oracle)
* pivotal-gemfire-9.0.2.zip (from Pivotal)

under gemfire-ansible, run the following commands to getup local gemfire cluster (docker):
```scripts/bootstrap
sudo ansible-galaxy install williamyeh.oracle-java
bash -c "clear && DOCKER_HOST=tcp://192.168.99.100:2376 DOCKER_CERT_PATH=~/.docker/machine/machines/default DOCKER_TLS_VERIFY=1 SSH_AUTHORIZED_KEYS=$(cat ${HOME}/.ssh/id_rsa.pub | base64 -i -) SSH_USER=gemfire /bin/bash"
ansible-playbook setup_gemfire.yml
```

## Verify Local GemFire Docker Containers

The Ansible playbook will create 4 docker containers, gf-locator1, gf-locator2, gf-server1, and gf-server2. To verify the provisioned containers, use the following command to ssh into each container.
```ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.2
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.3
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.4
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no gemfire@172.18.0.5
```

## Helpful Docker commands

```docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker container prune
```

## Uninstall Docker

```docker-machine rm -f default
brew cask uninstall --force docker-toolbox
brew cask uninstall --force docker
rm -f /usr/local/bin/docker
rm -f /usr/local/bin/docker-compose
rm -f /usr/local/bin/docker-machine
rm -rf ~/.docker
```
