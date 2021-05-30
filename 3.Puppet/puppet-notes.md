### ##############################
### Puppet Notes (Configuration Management)
### ##############################


### Agenda

- [x] Introduction
- [x] Puppet Server Installation
- [ ] Node Installation and Configuration
- [ ] Writing Simple Manifests
- [ ] Working Locally (puppet apply)
- [ ] Managing Nodes (Puppet agent run)
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

**Class Activity - Install and configure 1 or 3 node Puppet cluster**
```
- Refer to Class Activity 6.5 if you are on Simplilearn Lab
- Refer to "PuppetInstallation.txt" provided in GitHub repo if you are on AWS or DigitalOcean
```




















