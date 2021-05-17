### #########################
### Chef
### #########################


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


## Sample Chef Recipe to install and configure Apache
## myapacherecipe.rb

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







### References

    - https://downloads.chef.io/
