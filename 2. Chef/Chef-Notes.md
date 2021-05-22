### #########################
## Chef Notes
### #########################

### Agenda

- [x] Introduction
- [x] Installation and Configuration
    - [x] Chef Server installation
    - [x] Knife configuration [Chef Workstation]
    - [x] Bootstrapping a Chef Node
- [x] Understanding and Writing your first recipe
- [x] Talking to Chef Server via Knife [Knife commands]
- [x] Common Resources
- [ ] Writing and Working with Cookbooks
- [ ] Roles and Runlists
- [ ] Environments
- [ ] Databags


### Chef EcoSystem
    - Chef Automate
        - Chef Infra
        - Chef Habitat
        - InSpec
        - Chef Workstation


**Linux Family**

    - Redhat Family (RPM / Yum)
        - RHEL
        - CentOS
        - Fedora (Desktop)
        - Amazon Linux (Highly Customized version of CentOS 6)
        - Amazon Linux 2 (Highly Customized version of CentOS 7)

    - Debian Family (.deb / apt)
        - Ubuntu
        - Suse
        - Mint
        - Debian


### Sample Chef Recipe to install and configure Apache

**myapacherecipe.rb**

```
package 'httpd' do 
  action: install
end

file '/var/www/html/index.html' do
  content 'some content'
  mode '0755'
  owner 'apache'
end

service 'httpd' do
  action: start
end

```

### Chef Server Installation
```
sudo chef-server-ctl user-create sk12k Chef Admin chefadmin@sl.com 'Passw0rd' --filename ~/.chef/sk12k.pem


sudo chef-server-ctl org-create sl-org "Simplilearn DevOps" --association_user sk12k --filename sl-org.pem
```

### Chef Workstation
```
- Installation
    - https://downloads.chef.io/tools/workstation/current
- Knife Configuration
    - Download config.rb from Chef Org (Web UI) and place it in "chef-repo"

```

Test Kitchen Drivers
- Vagrant ---> virtualbox --> 
- 

### Get an alternate Chef environment (Class Activity)

```
- Register and create an Account on: https://manage.chef.io/
- Configure your Laptop as Chef workstation
    - Configure Knife using Starter Kit
        - Download Starter kit from Chef UI (Administrator page)
        - Unzip the kit to get "chef-repo"
    - Install ChefDK
        - https://downloads.chef.io/tools/workstation
        - Download the package as per your OS and complete the installation process
    - Validate
        - chef -v
        - Naviagate to "chef-repo" and run "knife node list" or knife cookbook list"
- Bootstrap DigitalOcean and/or AWS Nodes
    - knife bootstrap <fqdn> -U <username> -N <nodename> --sudo

    - knife bootstrap 138.68.173.255 -U root -N chef-demo-node --sudo
    - knife bootstrap 138.68.173.255 -i ~/.ssh/private_key -U root -N chef-demo-node --sudo

```

### Addiiontal Troubleshooting for SSH

```

passwd --> to change root password
passwd userx --> to change password for userx

SSH error: (for password based authentication)

edit the following file:

vi /etc/ssh/sshd_config

PasswordAuthentication no
to
PasswordAuthentication yes

Restart sshd
systemctl restart sshd

```

### Common Sysadmin tasks 
```
- Package management
- File/folder management
- user management
- service management
- security setting and updates
- network configuration
- server patching and maintenance
```
### Sample recipes and resources

**Install a Package**

```
vi install.rb

execute "apt-get update" do
    command "apt-get update"
end

package 'tree' do
    action :install
end

run the recipe locally using the following command:
chef-apply install.rb
```

**Install and configure Apache on Ubuntu**
```
1. Install apache package (apache2 or httpd)
2. Create a sample index.html file (/var/www/html/index.html)
3. Start and enable the service

vi apache.rb

## update apt-get
execute "apt-getupdate" do
  command "sudo apt-get update"
end

## Install apache2 package
package 'apache2' do
  action :install
end

## Create landing page"
file '/var/www/html/index.html' do
  content '<html>Hey there! My first chef recipe</html>'
  mode '0755'
end

## Start and enable the service
service 'apache2' do
 action [:start, :enable]
end

Validation:
- open page in web browser
- curl localhost
- check service status on node (systemctl status apache2)


```





### References
```
- https://downloads.chef.io/
- https://kitchen.ci/docs/getting-started/introduction/
- https://community.chef.io/tools/chef-inspec/


```