# Rapid Deployment Unit Testing Doc

This repo contains links and high level documentation on unit testing

## Overview

There are three levels of common testing:

### [Unit Testing](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#unit-testing-1)
* *rspec-puppet*: 
	1. tests the **catalogue** 
	2. [links](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#links)
	3. [high level overview](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#overview-1)

### [System Testing](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#system-testing-1)
* *serverspec/rspec-server*:
	1. Tests **machine configuration** via SSH to ensure resources you created from the catalogue are provisioned correctly.
	2. [links](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#links-1)
	3. [high level overview](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#overview-2)

### [Acceptance/Integration Testing](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#acceptanceintegration-testing)
* *beaker*/*beaker-rspec*:
	1. Full **integration testing** of many deployed machines
		* Deploys environment via VM's/cloud 
		* Configures machines 
		* Validates 
		* Tests for puppet resources and integration configuration
		* Destroys machines
		* Exits with report on deployment
	2. [links](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#links-2)
	3. [high level overview](https://github.com/puppetlabs/prosvcs_rapid_deploy_unit_testing_docs#overview-3)

## Unit Testing
### Links

* [rspec-puppet](http://rspec-puppet.com)
* [rspec puppet tutorial](http://rspec-puppet.com/tutorial/)
* [public git](https://github.com/rodjek/rspec-puppet) for rspec puppet

### Overview
Generally speaking unit enable you to **test how code behaves**. Literally, **in the context of Puppet, they are testing that resources declared in your manifests are in the compiled catalogue** and have valid parameters. In the context of testing functions, the unit test derives whether or not your function returns the correct value.

#### Install

```bash
gem install rspec-puppet
gem install puppet
```

#### Init

```bash
cd /path/to/your/module
rspec-puppet-init
```

## System Testing
### Links

* [serverspec](http://serverspec.org) website
* [serverspec](http://serverspec.org/tutorial.html) tutorial
* [public git](https://github.com/serverspec/serverspec0)

### Overview
System tests enable you to **test the state of current resources** as they exist on your local or remote system. Note, serverspec tests execute on an already provisioned box. This is one rung below acceptance/integration testing with Beaker which spins up temporary VM's to do what is basically a system test and exit then destroy. System testing with serverspec will test on **already existing** systems both locally and remotely via SSH.

#### Install

```bash
gem install serverspec
```

#### Init

```bash
cd /testing/directory
serverspec-init
```

## Acceptance/Integration Testing
### Links

**Project Websites**

* [Beaker Wiki](https://github.com/puppetlabs/beaker/wiki)
* [Beaker Git](https://github.com/puppetlabs/beaker)
* [Beaker-rspec Git](https://github.com/puppetlabs/beaker-rspec)

**How-To Videos**

* [Beaker 101, video](https://www.youtube.com/watch?v=cSyJXTYFXFg)

### Overview

RTFWiki
