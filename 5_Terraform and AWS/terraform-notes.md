### ###############################
### Managing AWS using Terraform
### ###############################


### Agenda

- [x] Introduction
- [x] Installation and configuration
- [x] Provisioning an EC2 instance
- [x] Provisioning VPC and Subnets


### Keywords
```
- IaC
- Hashicorp (Vagrant, consul, packer, terraform)
- Elastic Beanstalk (PaaS, Heroku)
```
### Introduction
```
- IaC
    - Terraform
    - AWS CloudFormation
    - Azure ARM templates
    - GCP Cloud Deployment Manager
```
**IaC benefits**
```
- Versioning
- Reproducible and reusable
- Scalable
- Auto Documented
- Consistency (less human error)
```
**Terraform Benefits**
```
- Multiple Providers and provisioners
- easy to learn HCL
```
**Terraform Licenses**
```
- Open Source
- Pro [Enterprise]
- Premium [Enterprise]
```

### Class activity - Terraform Installation
```
- Download zip file from https://www.terraform.io/downloads.html
- unzip the file
- Move the file to /usr/local/bin/
- terraform -v
```

**Terraform Workflow**
```
- terraform init  [Initialize_your_environment]
- terraform plan [Dry_run]
- terraform apply [Provision]
- terraform destroy [terminate_resources]
```
### Concepts
```
- Configuration files
- State
- Providers
- Provisioners
- Outputs
```
### AWS CLI installation steps
```
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"

sudo apt install unzip

unzip awscli-bundle.zip

sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

sudo /usr/bin/python3 awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

aws --version
```

**AWS CLI Configuration Steps**
```
aws configure
    AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
    AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
    Default region name [None]: us-west-2
    Default output format [None]: json
```
### Assignment
```
- create a VPC
- Create internet gateway and attach to the VPC
- Create a subnet
- Create security group to allow ports 22, 80, 443
- Create an EC2 instance (Windows, Ubuntu, CentOS)
- Install and configure Apache or IIS
```


### References:

- [Terraform Get Started - AWS](https://learn.hashicorp.com/collections/terraform/aws-get-started)

- [How To Use Ansible with Terraform for Configuration Management](https://www.digitalocean.com/community/tutorials/how-to-use-ansible-with-terraform-for-configuration-management)

