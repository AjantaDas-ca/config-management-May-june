### ##############################
### Puppet Notes (Configuration Management)
### ##############################


### Agenda

- [x] Introduction
- [x] Puppet Server Installation
- [x] Node Installation and Configuration
- [x] Writing Simple Manifests
- [x] Working Locally (puppet apply)
- [x] Managing Nodes (Puppet agent run)
- [x] Create and work with Puppet Modules
- [x] Create a cross platform module
- [x] Managing files in Puppet
- [x] Meta Parameters and Chaining arrows
- [x] Working with Modules from Puppet Forge


### keywords
- Bolt --> Push mode for Puppet
- Apache 2.0 / MIT --> Open Source (Examples: Linux and Git)
- Open Source != Free


**Anatomy of Puppet Agent run**
    
    - Runs the Facter locally to collect Facts
    - Sends facts to Puppet Server
    - Puppet Server compiles the catalogue
    - Puppet Agent pulls the catalogue
    - Agent applies the changes to the node as per the catalogue
    - Agent sends the updated report to Puppet Server

**Class Activity - 1 (Install and configure 1 or 3 node Puppet cluster)**
```
- Refer to Class Activity 6.5 if you are on Simplilearn Lab
- Refer to "PuppetInstallation.txt" provided in GitHub repo if you are on AWS or DigitalOcean
```

**Class Activity - 2 (Configure Local node using Puppet (puppet apply))**
```
## Install a Package
package { 'tree':
  ensure => 'present',
}

## Create a file
file { '/tmp/info.txt':
  ensure  => 'present',
  content => "This file was created using puppet on ${::fqdn}\n",
  mode    => '0644',
}

## Create a Directory
file { '/tmp/infodir':
  ensure  => 'directory',
}

## Create a user
user { 'sk12k':
  ensure   => 'present',
  password => 'Pa$$w0rd',
  home     => '/home/sk12k',
  shell    => '/bin/bash',
}

- Save the file with .pp extension
- run the file using the following command:
    puppet apply <filename.pp>

- validate the changes

```
**site.pp and init.pp**

```
Site.pp example

node default {
  For all nodes in the environment
}

node <nodeX> {
  applicable for nodeX
}

node webserver {
  applicable for all webservers
}

node dbservers {
  applicable for all db servers
}

node regex {
  applicable to all nodes fulfilling the regex
}
```
webserver --> default+webserver
dbserver --> default+dbserver
undefined --> default

```
init.pp example

```
**Class Activity - 3 (Apply changes to multiple nodes using 'Puppet Agent')**
```
create site.pp file at the following location:
/etc/puppetlabs/code/environments/production/manifests

vi site.pp

node default {
    ## Install a Package
    package { 'tree':
      ensure => 'present',
    }

    ## Create a file
    file { '/tmp/info.txt':
      ensure  => 'present',
      content => "This file was created using puppet on ${::fqdn}\n",
      mode    => '0644',
    }

    ## Create a Directory
    file { '/tmp/infodir':
      ensure  => 'directory',
    }

    ## Create a user
    user { 'sk12k':
      ensure   => 'present',
      password => 'Pa$$w0rd',
      home     => '/home/sk12k',
      shell    => '/bin/bash',
    }
}

- Apply the changes to nodes via "puppet agent -t"
- validate the changes
```

### Working with Puppet Modules

**Class Activity - 4 (Install PDK and create a Puppet Module)**

```
## Install PDK (Puppet development Kit)
apt-get update
apt-get install pdk -y
pdk --version

## Create a new Module
cd /etc/puppetlabs/code/environments/production/modules
pdk new module <module_name>
pdk new module webconfig
Note: You will be asked multiple questions regarding module metadata. Answer appropriately and save and exit.

```

**Class Activity - 5 (Create install class, update site.pp and run puppet agent)**
```
1. Create install class
cd /etc/puppetlabs/code/environments/production/modules/webconfig
pdk new class install

2. Edite the manifest file
vi /etc/puppetlabs/code/environments/production/modules/webconfig/manifests/install.pp

class webconfig::install {
  package { 'install_apache':
    name   => 'apache2',
    ensure => 'present',
  }
}

3. Check syntactical error
puppet parser validate manifests/install.pp

4. create init.pp file
pdk new class webconfig

5. edit init.pp file
vi /etc/puppetlabs/code/environments/production/modules/webconfig/manifests/init.pp

class webconfig {
  contain webconfig::install
}

save init.pp file and exit.

6. Edit the site.pp file to call the module
vi /etc/puppetlabs/code/environments/production/manifests/site.pp

node default {
  include webconfig
}

7. Run puppet agent on the nodes

8. Validate apache installation using curl command or via web browser
```

**Class Activity - 6 (Create a cross platform Apache installation Module)**
```
class webconfig::install {
  ## Define variable for package and service name
   $web_service = $facts['os']['family'] ? {
     'RedHat'  => 'httpd',
     'Debian'  => 'apache2',
   }
  ## Install package
   package { $web_service:
     ensure => 'present',
   }
  ## Start and enable the service
   service { $web_service:
     ensure => running,
     enable => true,
   }
}
```

**Class Activity - 7 (Managing files using Puppet)**
```
1. Create Config Class
pdk new class config

2. edit config.pp file
vi /etc/puppetlabs/code/environments/production/modules/webconfig/manifests/config.pp

class webconfig::config {
  file {'/var/www/html/index.html':
    ensure  => 'file',
    mode    => '0644',
    source  => 'puppet:///modules/webconfig/index.html',
  }
}

3. create source file in "files" directory of the module

vi /etc/puppetlabs/code/environments/production/modules/webconfig/files/index.html

4. Call the config manifest from init.pp

vi /etc/puppetlabs/code/environments/production/modules/webconfig/manifests/init.pp

class webconfig {
  contain webconfig::install
  contain webconfig::config
}

5. Run Puppet agent on nodes

6. validate the changes on Web browser (or using curl command)
```

### Meta Parameters and Chaining Arrows
```
- before
- require
- notify
- subscribe
- '->'
- '~>'
```
**Example Manifest for Meta-Parameters**
```
  ## Install package
  package { 'web':
     name  => $web_service,
     ensure => 'present',
     before => File['/var/www/html/index.html']
  }
  
  ## Start and enable the service
  service { 'web':
     name => $web_service,
     ensure => running,
     enable => true,
     subscribe => File['/var/www/html/index.html']
  }
  
  ## Create index.html file on nodes
  file {'/var/www/html/index.html':
    ensure  => 'file',
    mode    => '0644',
    source  => 'puppet:///modules/webconfig/index.html',
    require => Package['apache2']
    notify  => Service['apache2']
  }
```

**Example Manifest for Chaining Arrows**
```
  ## Install package
  package { 'web':
     name  => $web_service,
     ensure => 'present',
  }
  ## Create index.html file on nodes
->  file {'/var/www/html/index.html':
    ensure  => 'file',
    mode    => '0644',
    source  => 'puppet:///modules/webconfig/index.html',
  }

  ## Start and enable the service
~> service { 'web':
     name => $web_service,
     ensure => running,
     enable => true,
    }
```

### Assignments:
```
30-May-2021
  Complete the following exercises from Simplilearn LMS
    1. 6.5: Installing and Setting up Puppet on Ubuntu
    2. 6.9: Writing a Puppet Manifest
    3. 7.3: View Pre-Defined Puppet Resources
    4. 7.4: Create and Remove the Puppet Resources
    
5-June-2021
  - Practice the following walk-throughs on Puppet Forge documentation:
    1. https://puppet.com/docs/puppet/7/quick_start_essential_config.html
  - Complete the following exercises from Simplilearn LMS
    2. 7.6: Defining a Puppet Classes
    3. 7.10: Create a Puppet Module
    4. 7.11: Installing Module from Command Line
    5. 7.12: Manage Modules from Command Line
    6. 7.13: Install PHP in Agent Node
    7. 8.3: Installing Puppet Modules from Puppet Forge
    8. 8.4: Publish to Puppet Forge
    9. 8.11: Configuring hiera.yaml File
    10. 8.12: Testing Hiera
```
### References
```
- https://puppet.com/docs/puppet/6/config_file_main.html
- https://www.puppetcookbook.com/
- https://puppet.com/use-cases/windows-infrastructure-automation/
- https://learn.puppet.com/course/puppet-basics
- https://puppet.com/docs/puppet/7/quick_start_essential_config.html
- https://www.youtube.com/playlist?list=PLV86BgbREluX9pohlEYJ9eZrbWSBp6gET

```













