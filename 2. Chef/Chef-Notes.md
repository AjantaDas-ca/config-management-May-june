### #########################
## Chef Notes
### #########################

### Agenda

- [x] Introduction
- [x] Installation and Configuration
    - [x] Chef Server installation
    - [ ] Knife configuration [Chef Workstation]
    - [ ] Bootstrapping a Chef Node
- [ ] Understanding and Writing your first recipe
- [ ] Talking to Chef Server via Knife [Knife commands]
- [ ] Writing and Working with Cookbooks
- [ ] Common Resources
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


### References
```
- https://downloads.chef.io/



```