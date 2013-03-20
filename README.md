# Corundum

Corundum is a bare-bones Rails development VM for use with [Vagrant](http://vagrantup.com) and provisioned with [Chef](http://docs-v1.vagrantup.com/v1/docs/provisioners/chef_solo.html).

It aims to provide only the absolute bare essentials to get someone working on a Rails project on any Vagrant-supported platform.

### What you get:

The box is based on the precise64.box / Ubuntu 12.04 provided by Vagrant

- Rubies
  - rvm
  - ruby v1.9.3
  - ruby v2.0.0
- Databases
  - MySQL
  - PostgreSQL
- Javascript Runtime
  - Node

## Getting Started

Assuming you've already installed [Vagrant](http://downloads.vagrantup.com) 
and [VirtualBox](https://www.virtualbox.org/wiki/Downloads), 
in order to prepare corundum for use on your system:

1. Clone the repo

  ```
  git clone git://github.com/brrygrdn/corundum.git
  ```

2. Install gems

  ```
  bundle install
  ```
  
3. Fetch cookbooks

  ```
  librarian-chef install
  ```
    
4. Provision the VM

  ```
  vagrant up
  ```

5. Package the VM as a box

  ```
  vagrant package --vagrantfile Vagrantfile.pkg
  ```

  This will create package.box in your working directory. This file can be distributed to your team directly
  so they can skip steps 1-5.
  
6. Add the packaged VM to Vagrant for future use

  ```
  vagrant box add corundum package.box
  ```
    
7. Cleanup

  Before provisioning a corundum-based VM in one of your projects, make sure you tear down the VM used
  to create the packaged version by going to your working directory for this repo and running:
  
  ```
  vagrant destroy
  ```
    
  This ensures forwarded ports are free for new VMs.

### A note on Vagrantfile.pkg

This file contains some default ports and symlinks that are duplicated in the main Vagrantfile. 
Feel free to omit this if you would rather set these directly in your project's Vagrantfile.

## Using Corundum in a project

TODO

## Contributing

In order to contribute:

1. Fork the repo
2. Create a branch
3. Make your changes
4. Push your branch
5. Submit a pull request
