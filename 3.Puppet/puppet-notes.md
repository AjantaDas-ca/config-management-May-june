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
- [ ] Create and work with Puppet Modules
- [ ] Create a cross platform module
- [ ] Meta Parameters and Chaining arrows
- [ ] Managing files in Puppet
- [ ] Working with Modules from Puppet Forge


### keywords
- Bolt --> Push mode for Puppet

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

### Assignments:
```
30-May-2021
  Complete the following exercises from Simplilearn LMS
    1. 6.5: Installing and Setting up Puppet on Ubuntu
    2. 6.9: Writing a Puppet Manifest
    3. 7.3: View Pre-Defined Puppet Resources
    4. 7.4: Create and Remove the Puppet Resources
    


```

### References
```
- https://puppet.com/docs/puppet/6/config_file_main.html
- https://www.puppetcookbook.com/
- https://puppet.com/use-cases/windows-infrastructure-automation/
- https://learn.puppet.com/course/puppet-basics



```













