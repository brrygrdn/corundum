# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.host_name = "corundum"

  config.vm.network "forwarded_port", guest: 3000, host: 3030 # rails
  config.vm.network "forwarded_port", guest: 3306, host: 3307 # mysql
  config.vm.network "forwarded_port", guest: 5432, host: 5433 # postgresql

  config.vm.synced_folder ".", "/home/vagrant/code"

  config.vm.provision :chef_solo do |chef|
    # This path will be expanded relative to the project directory
    chef.cookbooks_path = "cookbooks"

    chef.add_recipe 'apt'
    chef.add_recipe 'build-essential'

    chef.add_recipe 'rvm::vagrant'
    chef.add_recipe 'rvm::system'
    chef.add_recipe 'rvm::user'

    chef.add_recipe 'nodejs'
    chef.add_recipe 'nodejs::npm'

    chef.add_recipe 'mysql::server'
    chef.add_recipe 'postgresql::server'

    chef.json = {
      rvm: {
        user_installs: [
          {
            version:      '1.18.21',
            user:         'vagrant',
            rubies:       ['1.9.3', '2.0.0'],
            default_ruby: '1.9.3',
            global_gems:  [
              { name: 'bundler'},
              { name: 'rake'}
            ],
            rvmrc: {
              rvm_project_rvmrc:             1,
              rvm_gemset_create_on_use_flag: 1,
              rvm_trust_rvmrcs_flag:         1
            }
          }
        ],
        vagrant: {
          system_chef_solo: '/opt/vagrant_ruby/bin/chef-solo'
        }
      },

      nodejs: {
        version: '0.10.0'
      },

      mysql: {
        version:                '5.5.29',
        server_root_password:   'password',
        server_repl_password:   'password',
        server_debian_password: 'password',
        allow_remote_root:      true          # access mysql root from remote (for development only)
      },

      postgresql: {
        version:          '9.1',
        listen_addresses: '*',
        password: {
          postgres: 'password'
        }
      }

    }

    config.vm.provision :shell, :inline => 'apt-get --yes install vim'
  end

end


