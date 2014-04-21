# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.define 'tomcat' do |tomcat|

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    tomcat.vm.network :private_network, ip: "192.168.4.20"

    tomcat.vm.network "forwarded_port", guest: 8080, host: 8080

    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    # config.vm.synced_folder "../data", "/vagrant_data"
    tomcat.vm.synced_folder '.', '/vagrant'
    tomcat.vm.synced_folder '/home/lino/tomcat', '/var/lib/tomcat7/webapps'

    tomcat.vm.provision :ansible do |ansible|
      ansible.groups = {
        "tomcats" => ["tomcat"]
      }
      ansible.playbook = "vagrant-main.yml"
      ansible.extra_vars = { user: "vagrant" }
      ansible.limit = 'all'
      ansible.host_key_checking = false
    end
  end
end