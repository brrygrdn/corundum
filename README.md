# Corundum

Corundum is a bare-bones Rails development VM for use with [Vagrant](http://vagrantup.com) and provisioned with [Chef](http://docs-v1.vagrantup.com/v1/docs/provisioners/chef_solo.html).

It aims to provide only the absolute bare essentials to get someone working on a Rails project on any Vagrant-supported platform.

### What you get:

The box is based on the precise64.box / Ubuntu 12.04 provided by Vagrant

- Rubies
  - rvm v1.18.21
  - ruby v1.9.3
  - ruby v2.0.0
- Databases
  - MySQL v5.5.29
  - PostgreSQL v9.1 ( Currently untested )
- Javascript Runtime
  - Node.js v0.10.0

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
  vagrant halt
  ```

  This ensures forwarded ports are free for new VMs.

### A note on Vagrantfile.pkg

This file contains some default ports and symlinks that are duplicated in the main Vagrantfile.
Feel free to omit this if you would rather set these directly in your project's Vagrantfile.

## Using Corundum in a project

To use corundum as a base-box for your project:

1. Add it to your available boxes in Vagrant

  ```
  vagrant box add corundum /path/to/corundum.box
  ```

  You can check which boxes you have installed with

  ```
  vagrant box list
  ```

2. Initialise Vagrant for your project

  ```
  cd my_project
  vagrant init
  ```

3. Edit the generated Vagrantfile to include the line

  ```
  config.vm.box = "corundum"
  ```

4. Provision your project's VM

  ```
  vagrant up
  ```

Make sure to add /.vagrant to your .gitignore so you don't checkin its cache file.

### Accessing the VM

Once provisioning is complete, access your VM by running ```vagrant ssh```

You can shut down the image with ```vagrant halt```

If you've made a mistake or are finished using the VM, run ```vagrant destroy``` to remove it.

You can open the VirtualBox Manager to see any VMs installed by Vagrant if in doubt.

### Where's my stuff?

Your working directory will be available inside your VM at ```~/code```

Rails, MySQL and other services are available on your host machine through a series of port-forwards.
You can see the default forwards in Vagrantfile.pkg.

As an example:

1. SSH into your VM

  ```
  vagrant ssh
  ```

2. Start a rails server

  ```
  cd code
  rails server
  ```

3. Point your browser at localhost:3030 to see your app.

### Customising

You can override many of the default settings of a new corundum-based box in your own project's Vagrantfile.
See examples/Vagrantfile for some hints.

Corundum only provides the basics for a Ruby / Rails project, you may need to provision some other depenancies yourself.
These can be added to your own Vagrantfile using chef / puppet or shell provisioning.

See the [Vagrant docs](http://docs-v1.vagrantup.com/v1/docs/provisioners.html) for more information.

If you are using Chef, I recomemend using [Librarian](https://github.com/applicationsonline/librarian)
to manage your cookbooks so you don't need to check them into your project or use git submodules.

## Contributing

In order to contribute:

1. Fork the repo
2. Create a branch
3. Make your changes
4. Push your branch
5. Submit a pull request
