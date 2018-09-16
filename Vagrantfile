VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 2.1.5"

require 'yaml'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = "bento/ubuntu-18.04"
	config.vm.box_check_update = true
	config.vm.provider "virtualbox" do |vm|
		vm.memory = 4096
		vm.cpus = 4
		vm.name = "devBox"
	end
	config.vm.provider "hyperv" do |vm|
		vm.memory = 1024
		vm.maxmemory = 8192
		vm.cpus = 4
		vm.vmname = "devBox"
	end
	
	# Open Ports and setting IP-address. (if you want, you can add the IP to your hosts file)
	# This setting will only be applied for VirtualBox. 
	# For HyperV you have to create your own network configuration and assign it during vagrant up
	config.vm.network :private_network, ip: "192.168.33.10"
	
	# Need to install docker before adding images, that proxys will be set.
	# If not doing so, the script will fail.
	config.vm.provision "docker" do |docker|
	end
	
	# Start docker container
	images = YAML.load_file('config/images.yml')
	config.vm.provision "docker" do |docker|
		images.each do |images|
			docker.run images["name"],
				image: images["image"],
				args: images["args"],
				cmd: images["cmd"]
		end
	end
end

