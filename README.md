# SEPIA Ansible Playbooks and Roles

This repository holds Ansible playbooks and roles for configuring SEPIA servers.

## Preparing a Destination Server

The destination server will need to be kickstarted with CentOS 7. Create a user called `sepia_ansible` on the destination server:

```bash
adduser sepia_ansible
passwd sepia_ansible
usermod -aG wheel sepia_ansible
```

Then edit `/etc/sudoers` and add these lines:

```
User_Alias        NO_SUDO_PASSWORD = sepia_ansible
NO_SUDO_PASSWORD  ALL = (ALL) NOPASSWD: ALL
```

You will then need to add your public key from your host control machine to `sepia_ansible`'s authorized keys.

## Getting started

Clone this repository.

    git clone git@github.com:FlipperPA/sepia-ansible.git
    cd sepia-ansible

Deploy the dev environment:

    ansible-playbook playbooks/web-py.yml

Show some servers:

    ansible centos-7 --list-hosts
    ansible web-py --list-hosts

Run some commands; `shell` is required for arguments and shell functions (`command` runs without the target user's shell):

    ansible all -m command -a uptime -i inventory
    ansible all -m shell -oa 'ps -eaf'
    ansible rhel-7 -m copy -a 'content="Welcome to SEPIA, Ansiblized.\n" dest=/etc/motd' -u apache --become --become-user root

Documentation: list help section, show documentation for specific command (with examples!):

    ansible-doc -l
    ansible-doc yum
