## #########################

### Keywords

- State of DevOps Report
- Password-less Authentication
- Configuration Drift
- Out-of-Band Changes
- Desired State Configuration
- Domain Specific Language
    - Terraform --> HCL
    - Chef --> DSL
    - Puppet --> DSL
    - Ansible --> YAML
    - Cloudformation --> JSON/YAML
    - Jenkins --> Groovy
  



Golden Template
.net 4.5
user x read permission on folder y
port 80, 443


.net 5.0
user x r/w permission on folder y
port 80, 443, 5985



3 ways to talk to AWS:
  - Console (GUI)
  - AWS CLI (Command Line)
  - SDK (Programmatic Way)

Chef - 3
Puppet - 3
Ansible - 3
AWS Fundamentals - 1
Terraform - 1
Project - 1


- Password-less Authentication
    - Private + Public Key pair
    - Known Hosts
    - Authorized Keys


ssh-copy-id -i id_rsa.pub user@hostname


ssh -i .ssh/id_rsa root@<ipaddress>

### SSH client recommendation

- Macbook
    - Termius
    - iterm2

- Windows
    - Putty
    - MobaxTerm
    - WSL
    - Cygwin

### Text Editor
    - VS Code
    - Sublime
    - Atom
    - Notepad++    
    - Eclipse
    - IntelliJ
    - PyCharm


### Configuration Management vs Infrastructure-as-Code

- Infrastructure-as-Code (Automated Provisioning)
    - Terraform | HCL
    - CloudFormation (AWS) | JSON/YAML
    - ARM Templates (Azure) | JSON
    - Heat Templates (OpenStack) | DSL

- Configuration Management (Managing/Maintaing the State)
    - Ansible
    - Chef
    - Puppet
    - CFEngine
    - BladeLogic
    - Powershell DSC
    - SaltStack






### References:

- https://linuxjourney.com/
