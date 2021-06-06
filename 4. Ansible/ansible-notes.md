### ##############################
### Ansible Notes (Configuration Management)
### ##############################


### Agenda

- [ ] Introduction
- [ ] Ansible controller Installation
- [ ] ssh configuration
- [ ] Inventory and Host file configuration
- [ ] Ad Hoc Commands
- [ ] Playbooks
    - [ ] YAML
    - [ ] Plays
    - [ ] Tasks
- [ ] Modules
- [ ] Loops and Conditions
- [ ] Roles
- [ ] Galaxy
- [ ] Ansible for AWS
    - [ ] EC2
    - [ ] S3


### Installation and Configuration

**Class Activity 1 - (Ansible controller Installation)**

```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

**Validation**
ansible --version
```
### Passwordless SSH configuration between Controller and Nodes
**Class Activity - 2A (Create a System Account)**
```
adduser ansible
sudo usermod -aG sudo ansible (for CentOS ==> usermod -aG wheel ansible)
visudo (to edit /etc/sudoers)
'%sudo   ALL=(ALL:ALL) ALL'  ---> '%sudo   ALL=(ALL:ALL) NOPASSWD:ALL'
```

**Class Activity 2B (Setup password-less Authentication via ssh)**
```
su - ansible
ssh-keygen -t rsa ==> this step will create pulic/private key pair in ~/.ssh/ directory

.ssh
├── id_rsa
├── id_rsa.pub

Syntax:
ssh-copy-id -i ~/.ssh/id_rsa.pub <username>@<IPAddress>

Example:
ssh-copy-id -i ~/.ssh/id_rsa.pub ansible@159.65.51.250

Note: you might have to edit sshd_config file on the nodes for Password based authentication.

vi /etc/ssh/sshd_config

change
PasswordAuthentication no
to
PasswordAuthentication yes

**Restart the service:
sudo systemctl restart sshd
sudo service sshd restart

```


