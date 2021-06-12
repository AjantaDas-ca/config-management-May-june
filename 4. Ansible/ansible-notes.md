### ##############################
### Ansible Notes (Configuration Management)
### ##############################


### Agenda

- [x] Introduction
- [x] Ansible controller Installation
- [x] ssh configuration
- [x] Inventory and Host file configuration
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

### Ansible commands
- ansible           ---> Ad Hoc Commands
- ansible-playbook  ---> Work with Playbooks
- ansible-galaxy    ---> Work with Roles


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

Change:
'%sudo   ALL=(ALL:ALL) ALL'
to
'%sudo   ALL=(ALL:ALL) NOPASSWD:ALL'
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
**Class Activity - 3 (Modifying Ansible Inventory (via /etc/ansible/hosts))**
```
Sample file. Please make the changes and update your local hosts file inside /etc/ansible directory.

[nodes]
u-node ansible_host=178.62.122.74
c-node ansible_host=159.65.51.250

[centos]
159.65.51.250

[ubuntu]
178.62.122.74

[webservers]
178.62.122.74
159.65.51.250

[allnodes:children]
centos
ubuntu

**Validation**

ansible <group-name> -m ping

ansible --list-hosts <group_name>
```

**Class Activity - 4 (Alternate Inventory File)**

```
Ansible gives you the ability to provide alternate files as inventory files and you don't always have to rely on /etc/ansible/hosts.

## Create a file with node details which you want to use as alternative inventory
1. vi /etc/ansible/webinv

[newinv]
178.62.122.74
159.65.51.250

Save the file and exit.

2. run ansible commands with -i flag

Syntax:
ansible <group_name> -m <module_name> -i <inventory_file_path>

Example:
ansible newinv -m ping -i /etc/ansible/webinv
```
_Additional Note: if you want to make the new inventory file persistent so that you don't have to use the -i flag, you can change the corresponding value in the "defaults" section of main Ansible configuration file (typically '/etc/ansible/ansible.cfg')_

### Ad Hoc Commands (ansible ---options)

**check node health**

    ansible all -m ping

**List ansible nodes**

    ansible --list-hosts all
    ansible --list-hosts <node-group-name>

**See all the facts on nodes**

    ansible all -m setup

**Run shell commands on nodes**

    ansible nodes -m shell -a 'ls /tmp'

**Run command with sudo**

    ansible nodes -m shell -a 'fdisk -l' --become

**Create a user**

    ansible nodes -m user -a 'name=sk12k gid=1003 uid=1003 password=123456' --become

**Install a package**

    ansible nodes -m apt -a 'name=tree state=present' --become ## For Ubuntu
    ansible nodes -m yum -a 'name=tree state=present' --become ## CentOS
    ansible nodes -m package -a 'name=tree state=present' --become ## Generic

**Uninstall a package**

    ansible nodes -m package -a 'name=tree state=absent' --become ## Generic

**Create a file**

    ansible nodes -m copy -a 'src=info.txt dest=/tmp/info.txt owner=root mode=0644' --become

**Start/stop a service**

    ansible nodes -m service -a 'name=<service-name> state=started' ## Start a service
    ansible nodes -m service -a 'name=<service-name> state=stopped' ## Stop a service

### Working with Ansible Playbooks (ansible-playbook ---options)

ansible all -m ping  --become
**ping.yaml**
```
---
- hosts: all
  remote_user: ansible
  become: yes
  tasks:
    - name: Ping all nodes in my infra
      action: ping

Run: ansible-playbook ping.yaml
```


### Assignments
```
6-June-2021
    1. Setup a 3-node cluster as shown during class demo for practice.
    2. Classroom activities 1-4
    3. 10.3: Ansible Modules (Simplilearn LMS)
    4. Read about inventory file (Reference link)

```

### References:
- https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html
- https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html
- 
