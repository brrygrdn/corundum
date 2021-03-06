# Corundum

Corundum is a bare-bones [Vagrant](http://vagrantup.com) box for
Ruby / Ruby on Rails projects.

It uses [VirtualBox](http://docs.vagrantup.com/v2/virtualbox/index.html) as
a provider and is provisioned with
[Chef](http://docs.vagrantup.com/v2/provisioning/chef_solo.html).

It aims to provide only the absolute bare essentials to get someone working
on a Rails project on any Vagrant/VirtualBox-supported platform.

### What you get:

The box is based on the precise64.box (Ubuntu 12.04) provided
by Vagrant (see: https://vagrantcloud.com/hashicorp)

Installs:
- Latest RVM
- git
- oh-my-zsh
- Ruby v2.1.1
- MySQL v5.5.29
 - Password is defaulted to 'vagrant' for root user

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

  This will create package.box in your working directory. This file can be
  distributed to your team directly so they can skip steps 1-5.

  The Vagrantfile.pkg ensures the box is packaged with network configuration
  and folder syncing that would always need to be in the client Vagrantfile

6. Add the packaged VM to Vagrant for future use

  ```
  vagrant box add package.box --name brrygrdn/corundum
  ```

7. Cleanup

  Before provisioning a corundum-based VM in one of your projects, make
  sure you tear down the VM used to create the packaged version
  by going to your working directory for this repo and running:

  ```
  vagrant halt
  ```

  This ensures forwarded ports are free for new VMs.

## Using Corundum in a project

To use corundum as a base-box for your project:

1. Add it to your available boxes in Vagrant

  ```
  vagrant box add brrygrdn/corundum
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
  config.vm.box = "brrygrdn/corundum"
  ```

4. Provision your project's VM

  ```
  vagrant up
  ```

Make sure to add /.vagrant to your .gitignore so you don't checkin
its cache file.

### Accessing the VM

Once provisioning is complete, access your VM by running ```vagrant ssh```

You can shut down the image with ```vagrant halt```

If you've made a mistake or are finished using the VM, run ```vagrant destroy```
to remove it.

You can open the VirtualBox Manager to see any VMs installed by
Vagrant if in doubt.

### Where's my stuff?

Your working directory will be available inside your VM at ```~/my_project```

Rails, MySQL and other services are available on your host machine through
a series of port-forwards.
You can see the default forwards in Vagrantfile.pkg.

As an example:

1. SSH into your VM

  ```
  vagrant ssh
  ```

2. Start a rails server

  ```
  cd my_project
  rails server
  ```

3. Point your browser at localhost:3030 to see your app.

### Customising

You can override many of the default settings of a new corundum-based box in
your own project's Vagrantfile.
See examples/Vagrantfile for some hints.

Corundum only provides the basics for a Ruby / Rails project, you may need to
provision some other depenancies yourself.
These can be added to your own Vagrantfile using chef / puppet or
shell provisioning.

See the [Vagrant docs](https://docs.vagrantup.com/v2/provisioning/index.html)
for more information.

If you are using Chef, I recomemend using
[Librarian](https://github.com/applicationsonline/librarian-chef)
to manage your cookbooks so you don't need to check them into your
project or use git submodules.

## Contributing

In order to contribute:

1. Fork the repo
2. Create a branch
3. Make your changes
4. Push your branch
5. Submit a pull request
