### ###############################
### Managing AWS using Ansible and Terraform
### ###############################


- [x] Dynamic Inventory [AWS]
- [ ] Ansible for AWS
    - [x] EC2
    - [x] S3
- [ ] Terraform
    - [ ] Introduction
    - [ ] Installation and configuration
    - [ ] Provisioning an EC2 instance
    - [ ] Provisioning an S3 Bucket


### Keywords

- Ansible --> Config Management
- Terraform --> Infrastructure-as-Code

Terraform + Ansible
CloudFormation + Systems Manager
#### Class Activity - Working with Dynamic Inventory in Ansible (30 Minutes)

**Installing Boto3 botocore**
```
sudo apt-get install python3 -y

sudo apt-get install python3-pip

pip install boto
pip install boto3
pip install boto3
pip3 install boto3
```

**Edit the ansible.cfg file**
```
sudo vi /etc/ansible/ansible.cfg

under the [inventory] section add the following line:

enable_plugins = aws_ec2
```

**create the credentials fils**
```
vi aws_ec2.yaml

---
plugin: aws_ec2
aws_access_key: <your-access-key>
aws_secret_key: <your-secret-key>
keyed_groups:
  - key: tags
    prefix: tag
```
**Validate**
```
ansible-inventory -i aws_ec2.yaml --graph
aws_ec2.yaml --graph
```

**Modify the credentials fils to group the outputs**
```
vi aws_ec2.yaml

---
plugin: aws_ec2
aws_access_key: <your-access-key>
aws_secret_key: <your-secret-key>
keyed_groups:
  - key: tags
    prefix: tag
  - prefix: instance_type
    key: instance_type
  - key: placement.region
    prefix: aws_region
```

**Validate**
```
Get the list of Nodes by groups:
ansible-inventory -i aws_ec2.yaml --graph

Ping specific set of nodes as per the group:
ansible aws_region_eu_west_1 -m ping -i aws_ec2.yaml

Run a Playbook on taget nodes:
ansible-playbook <group-name> -i aws_ec2.yaml playbook.yaml
```

#### Class Activity - Creating an EC2 instance using Ansible (15 Minutes)

**Write a playbook to provision and EC2 instance**
```
---
- hosts: localhost
  gather_facts: no
  vars_files:
    - keys.yaml
  tasks:
    - name: Provision_EC2
      ec2:
        aws_access_key: "{{ AWS_ACCESS_KEY_ID }}"
        aws_secret_key: "{{ AWS_SECRET_ACCESS_KEY }}"
        aws_region: "{{ AWS_REGION }}"
        keypair: sk12k-us-east1
        instance_type: t2.micro
        image: ami-09e67e426f25ce0d7 #Ubuntu 20.04
        wait: yes
        vpc_subnet_id: subnet-2c626620
        assign_public_ip: yes
        instance_tags:
          Name: ansible-demo
```

**Create AWS credentials file for AWS**
```
--- # AWS Credentials for Ansible
AWS_ACCESS_KEY_ID: <your-access-key>
AWS_SECRET_ACCESS_KEY: <your-access-key>
AWS_REGION: <aws-region>
```

**Terminate/stop the instances**

```
---
- hosts: localhost
  gather_facts: no
  vars_files:
    - keys.yaml
  vars:
    instance_ids:
      - 'i-0973edc48fbba34a6'
      - 'i-0b23553f11f209359'
      - 'i-08830a8d725a0216a'
  tasks:
    - name: Stop_EC2_instances
      ec2:
        aws_access_key: "{{ AWS_ACCESS_KEY_ID }}"
        aws_secret_key: "{{ AWS_SECRET_ACCESS_KEY }}"
        aws_region: "{{ AWS_REGION }}"
        instance_ids: '{{ instance_ids }}'
        state: terminated
   
```

### References

- https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html
