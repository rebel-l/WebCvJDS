# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	# Setup for all machines
	config.vm.provision "shell", inline: "echo Starting Vagrant ..."
	config.vm.box = "Ubuntu14.04LTS"
	config.vm.box_url = "https://www.dropbox.com/s/gzbxpgjih67uu2t/ubuntu1404lts5018.box?dl=1"
	config.ssh.insert_key = false	# Avoid that vagrant removes default insecure key

	# Host manager setup
    config.hostmanager.enabled				= true
    config.hostmanager.manage_host			= true
    config.hostmanager.manage_guest			= true
    config.hostmanager.ignore_private_ip	= false
    config.hostmanager.include_offline		= true

	# Setup for WebCvJDS machine only
	config.vm.define "WebCvJDS", autostart: true do |webcvjds|
		webcvjds.vm.network "private_network", ip: "192.168.34.10"
		webcvjds.vm.hostname = 'service.jds.dev'

		# Provider-specific configuration
		config.vm.provider "virtualbox" do |vbjds|
		    vbjds.name = "WebCvJds_Service"
			vbjds.memory = 384
#			vbjds.gui = true
		end

		# Chef Configuration
		webcvjds.vm.provision "chef_solo" do |chef|
			chef.cookbooks_path = "./vendor/rebel-l/sisa/cookbooks"
			chef.roles_path = "./vendor/rebel-l/sisa/roles"
			chef.environments_path = "./vendor/rebel-l/sisa/environments"
			chef.data_bags_path = "./vendor/rebel-l/sisa/data_bags"
			chef.add_role "Default"
			chef.environment = "development"
			chef.add_recipe "GolangCompiler"
			chef.add_recipe "Memcached"

			chef.json = {
				'Golang' => {
					'version' => '1.6.2',
					'gopath' => '/home/vagrant/go',
					'packages' => [
						'github.com/gin-gonic/gin'
					]
				},
				'Iptables' => {
					'Whitelist'	=> {
						'Ports' => [8081]
					}
				}
            }
		end
	end

	# Setup for MongoDb machine only
	config.vm.define "MongoDb", autostart: true do |mongodb|
		mongodb.vm.network "private_network", ip: "192.168.34.20"
		mongodb.vm.hostname = 'mongodb.jds.dev'

		# Provider-specific configuration
		config.vm.provider "virtualbox" do |vbmongo|
		    vbmongo.name = "WebCvJds_MongoDb"
			vbmongo.memory = 1024
			vbmongo.cpus = 2
#			vbmongo.gui = true
		end

		# Chef Configuration
		mongodb.vm.provision "chef_solo" do |chef|
			chef.cookbooks_path = "./vendor/rebel-l/sisa/cookbooks"
			chef.roles_path = "./vendor/rebel-l/sisa/roles"
			chef.environments_path = "./vendor/rebel-l/sisa/environments"
			chef.data_bags_path = "./vendor/rebel-l/sisa/data_bags"
			chef.add_role "DbMongo"
			chef.environment = "development"

			chef.json = {
                'Iptables' => {
                    'Whitelist'	=> {
                        'Ports' => [27017, 28017]
                    }
                }
            }
		end
	end
end
