# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "hashicorp/precise64"

  config.vm.host_name = "corundum"

  config.vm.network "forwarded_port", guest: 3000, host: 3030 # rails
  config.vm.network "forwarded_port", guest: 3306, host: 3307 # mysql

  config.vm.synced_folder ".", "/home/vagrant/code"

  config.vm.provision :chef_solo do |chef|
    # This path will be expanded relative to the project directory
    chef.cookbooks_path = "cookbooks"

    chef.add_recipe 'apt'
    chef.add_recipe 'build-essential'

    chef.add_recipe 'mysql::server'

    chef.json = {
      mysql: {
        version:                '5.5.29',
        data_dir:               '/var/lib/mysql',
        port:                   '3306',

        server_root_password:   'vagrant',
        server_repl_password:   'vagrant',
        server_debian_password: 'vagrant',

        allow_remote_root:      true, # access mysql root from remote (for development only)
        remove_anonymous_users: true
      }
    }

    config.vm.provision :shell, inline: 'apt-get --yes install vim'
    config.vm.provision :shell, inline: 'apt-get --yes install curl'

    config.vm.provision :shell, path: 'scripts/install-rvm.sh',  args: 'stable', privileged: false
    config.vm.provision :shell, path: 'scripts/install-ruby.sh', args: '2.1.1', privileged: false
  end

end
