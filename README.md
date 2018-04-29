# CentOS 7 Python 3 & Django Ansible Playbooks and Roles

This repository holds Ansible playbooks and roles for configuring Python 3 & Django servers. You should be able to clone the repository and follow these steps to get a server up and running; this has been tested with Digital Ocean droplets.

You'll want to come up with a service account name that you'll use in place of `your_project_ansible_user`. After cloning this repository, edit `ansible.cfg` and change `remote_user = your_project_ansible_user` with the username you will use.

## Preparing a Destination Server

The destination server will need to be kickstarted with CentOS 7. Log in as root to your freshly created Digital Ocean droplet (you used SSH keys, right?), and create a user called `your_project_ansible_user` on the destination server:

```bash
adduser your_project_ansible_user
passwd your_project_ansible_user
usermod -aG wheel your_project_ansible_user
chmod 640 /etc/sudoers
```

Then edit `/etc/sudoers`. Comment out the line that says `# %wheel        ALL=(ALL)       ALL` and uncomment the line that says `%wheel  ALL=(ALL)       NOPASSWD: ALL`. Then let's become the `your_project_ansible_user` user, and generate keys:

```bash
su - your_project_ansible_user
ssh-keygen
```

You will then need to add your public key from your host control machine to `your_project_ansible_user`'s authorized keys in `~/.ssh/authorized_keys`:

```bash
echo "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAv+IdIP7geRsOXUz7scIKTwkfxJ3J7UelRObTifdnmCHR6s53+irm9uVBP8se2IH1Vw4ETfjtdzI4vHce1lYwFVQx9V9cmKFyxCnroV7bTx6EPQ2WqkpAOfM5IGG7vGnrX3M0MPLrA0T8XrhPpCzM2GfNR8fqOQiPROP5blahblahblahblahblah5TQknqu7/twBtXuMpKakR4Vo08cq1MBJI8akEG/tzppoeYuRY8BzKqJVD+2Gp1RgBVsXLxX2W9ng6cFAHTRxs65koTWyVLCCCXsP54X4UqJRy1x5/PumL1VJn8LvXTGcOyfyFSBLClQ== you@yourdomain.com" > .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```

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
