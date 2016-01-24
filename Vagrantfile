# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	# Setup for all machines
	config.vm.provision "shell", inline: "echo Starting Vagrant ..."
	config.vm.box = "Ubuntu14.04LTS"
	config.ssh.insert_key = false	# Avoid that vagrant removes default insecure key

	# Setup for WebCvJDS machine only
	config.vm.define "WebCvJDS", autostart: true do |webcvjds|
		webcvjds.vm.network "private_network", ip: "192.168.34.10"

		# Provider-specific configuration
		config.vm.provider "virtualbox" do |vb|
			vb.memory = 384
#			vb.gui = true
		end

		# Chef Configuration
		webcvjds.vm.provision "chef_solo" do |chef|
			chef.cookbooks_path = "./vendor/rebel-l/sisa/cookbooks"
			chef.roles_path = "./vendor/rebel-l/sisa/roles"
			chef.environments_path = "./vendor/rebel-l/sisa/environments"
			chef.data_bags_path = "./vendor/rebel-l/sisa/data_bags"
			chef.add_role "Default"
			chef.environment = "development"
		end
	end

	# Setup for MongoDb machine only
	config.vm.define "MongoDb", autostart: true do |mongodb|
		mongodb.vm.network "private_network", ip: "192.168.34.20"

		# Provider-specific configuration
		config.vm.provider "virtualbox" do |vb|
			vb.memory = 1024
			vb.cpus = 2
#			vb.gui = true
		end

		# Chef Configuration
		mongodb.vm.provision "chef_solo" do |chef|
			chef.cookbooks_path = "./vendor/rebel-l/sisa/cookbooks"
			chef.roles_path = "./vendor/rebel-l/sisa/roles"
			chef.environments_path = "./vendor/rebel-l/sisa/environments"
			chef.data_bags_path = "./vendor/rebel-l/sisa/data_bags"
			chef.add_role "DbMongo"
			chef.environment = "development"
		end
	end
end
