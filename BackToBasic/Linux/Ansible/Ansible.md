title: Ansible
date: 2017-07-20 16:10:33
tags:
- linux
- ansible
- shell
- automation
---

# setup ansible

http://docs.ansible.com/ansible/latest/intro_getting_started.html

## setup and ping all the hosts

To set up the ssh

1) generate key pair

 ```shell
ssh-keygen -t rsa -f /<path-to-key>/authorized_keys.myuserid
```
 * After this command, we should get one pair of keys
 * myuserid should be the user exist on all the target servers and have access

2) add private key to ssh-client on control box

 ```shell
ssh-agent bash
ssh-add /<path-to-key>/authorized_keys.myuserid
```

3) list all hosts into /etc/ansible/hosts file

4) using myuserid to log in control machine, and start triggerring command

 ```shell

ansible all -m ping -u liurx5 -i ./DEV.hosts --sudo --ask-become-pass
ansible all -a "/bin/echo hello" --sudo --ask-become-pass
```

## run playbook against target hosts as root

 ```shell
ansible-playbook checkRequiredApps.yml -i ./hosts --ask-become-pass
```

## Task Samples
