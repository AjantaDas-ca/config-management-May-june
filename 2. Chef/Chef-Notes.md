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
- [x] Writing and Working with Cookbooks
- [x] Roles and Runlists
- [x] Templates
- [x] Environments
- [x] Databags
- [x] Chef Supermarket


### Chef EcoSystem
    - Chef Automate
        - Chef Infra
        - Chef Habitat
        - InSpec
        - Chef Workstation

### Important files in Chef
    - knife.rb (or config.rb)
    - user.pem
    - client.pem

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

**Recipe Syntax**

```
type 'name' do
    action :type_of_action
    attribute 'value'
end
```
### Class Activity (Create a recipe and run locally on node)

```
## Create a user

user 'sk12k' do
  comment 'A random user'
  uid 1234
  home '/home/sk12k'
  shell '/bin/bash'
  password 'Passw0rd'
end

## Create a file on target node
file '/tmp/info.txt' do
  content 'This file was created using Chef local mode'
end

## Install a package

package 'tree'


## Delete/remove a user

user 'sk12k' do
  action :remove
end

```

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

- bootstrapping using password
    - knife bootstrap 138.68.173.255 -U root -N chef-demo-node --sudo

- bootstrapping via passwordless authentication   
    - knife bootstrap 138.68.173.255 -i ~/.ssh/private_key -U root -N chef-demo-node --sudo

- Bootstrapping with a role pre-assigned
    - knife bootstrap 46.101.61.133 -U root -N chefnode1 -r 'role[new_webservers]' --sudo

```
**Note: When you bootstrap a windows node, please open port 5985/5986 for WinRM**

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

### Common Chef Resources
```
- package
- service
- file
- cookbook_file
- windows_package
- template
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

**Class Activity (Generate "web" Cookbook on Workstation and upload on Chef server)**
```
1. Create a cookbook using chef generate command

chef generate cookbook <cookbookname>

2. edit the recipe

vi recipe/default.rb

## Install Apache
package 'apache2' do
    action :install
end

## Start and enable Apache service
service 'apache2' do
    action [:start, :enable]
end

3. upload the cookbook
knife cookbook upload <cookbookname>

4. Edit node's runlist

5. Run "sudo chef-client" on the node

6. Validate by accessing the web page in a browser
```
**Class Activity (Create "motd" Cookbook and work with cookbook_file resource)**
```
1. Generate coobook:
chef generate cookbook motd

2. Edit default recipe:
vi cookbooks/motd/recipes/default.rb

cookbook_file "/etc/motd" do 
    source "motd"
    mode "0644"
end

3. Generate Source file:
chef generate file motd

4. Edit file with desired content:
vi cookbooks/motd/files/motd

Put the following content inside "motd" file:

##########################################
##########################################
This Node is being managed by Chef, Don't 
dare to make any Out-of-band changes.
Violaters would be brought to justice!!
##########################################
##########################################

5. Update the run-list/role for the node and run chef-client
```

**Class Activity (Bootstrap a node with pre-existing Role)**

```
Create a new instance (on AWS or DO) and bootstrap with an existing role:

- Bootstrapping with a role pre-assigned
    - knife bootstrap 46.101.61.133 -U root -N chefnode1 -r 'role[new_webservers]' --sudo

```
### Chef Environments


**Class Activity - Working with Chef Environments**

```
1. Create a new environment using rb file method
    a. create an rb file ~chef-repo/environments/prod.rb
    b. knife environment from file ~chef-repo/environments/prod.rb
2. Add a node to the new environment
3. Create a new version of "web" cookbook
    a. Update version in ~chef-repo/cookbooks/webserver/metadata.rb from '0.1.0' to '0.2.0'
    b. knife cookbook upload web
4. Add constraint for cookbook in your environment
5. Run chef-client
6. Validate the changes
```



**Assignment**

```
16-May-2021
    1. Complete Chef Server Installation and Workstation configuration (Exercise 3.4)
22-May-2021
    2. Bootstrap an AWS node from Simplilearn lab to your manage.chef.io account.

23-May-2021
    3. Install and configure Puppet Cluster using the Installation document provided

29-May-2021
    4. template
    5. databags
    6. supermarket

```

### References
```
- https://downloads.chef.io/
- https://kitchen.ci/docs/getting-started/introduction/
- https://community.chef.io/tools/chef-inspec/
- https://docs.chef.io/resources/
- learn.chef.io
- https://learn.chef.io/courses/course-v1:chef+Infra101+perpetual/course/
- https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/
- https://www.verisign.com/en_US/website-presence/online/how-dns-works/index.xhtml
- 

```