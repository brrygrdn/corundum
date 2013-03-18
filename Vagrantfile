# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  # TODO: Reconsider base box
  #
  # Current precise64 has VirtualBox guest additions v4.2.0, current VirtualBox v4.2.8
  # Warns on machine up, but does not disrupt port forwarding or shared folders
  #
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.forward_port 3000, 3000

  config.vm.share_folder "code", "/home/vagrant/code", ".", :create => true

  config.vm.provision :chef_solo do |chef|
    # This path will be expanded relative to the project directory
    chef.cookbooks_path = "cookbooks"

    chef.add_recipe 'apt'
    chef.add_recipe 'build-essential'

    chef.add_recipe 'rvm::vagrant'
    chef.add_recipe 'rvm::system'
    chef.add_recipe 'rvm::user'

    chef.json = {

      rvm: {
        user_installs: [
          {
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
      }

    }
  end

end


