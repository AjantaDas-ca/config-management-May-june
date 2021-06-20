### ######################
### Project 1 
### CI/CD Deployment Using Ansible CM Tool 
### ######################

**Steps to Perform:**
```
Step 1: Configure Jenkins server as Ansible provisioning machine
  - Install Jenkins and Ansible on your node

Step 2: Install Ansible plugins in Jenkins CI server
  - Install appropriate Ansible plugin for your Jenkins CI server

Step 3: Prepare Ansible playbook to run Maven build on Jenkins CI server
  - use shell module to run maven commands using Ansible

Step 4: Prepare Ansible playbook to execute deployment steps on the remote web container with restart of the web container post deployment
  - Write playbook for deployment and container restart
```
**Desired Workflow:**
```
Jenkins ---> Ansible --> Maven build ---> war file deployment (Jenkins post build steps or using Ansible playbook)
```

### ######################
### Project 2
### Deployment of WordPress Environment
### ######################
```
WordPress installation and configuration with the following components:

    PHP
    Nginx/Apache Web Server
    MySQL
    WordPress


Remark/Tip: use appropriate Ansible role.
```
