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
**Passwordless SSH configuration**




