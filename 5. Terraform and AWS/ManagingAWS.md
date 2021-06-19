### ###############################
### Managing AWS using Ansible and Terraform
### ###############################


- [ ] Dynamic Inventory [AWS]
- [ ] Ansible for AWS
    - [ ] EC2
    - [ ] S3
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
#### Dynamic Inventory


**Installing Boto3 botocore**
```
sudo apt-get install python3 -y

sudo apt-get install python3-pip

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
aws_access_key: AKIARGOSPLBUGKCBZ2D3
aws_secret_key: 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
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










**Write a playbook to provision and EC2 instance**
```
---
- hosts: localhost
  gather_facts: no
  tasks:
    - name 




### References

- https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html
