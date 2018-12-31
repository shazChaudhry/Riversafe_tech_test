# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
	# https://app.vagrantup.com/centos/boxes/7
	config.vm.box = "centos/7"
	config.hostmanager.enabled = true
	config.hostmanager.manage_host = true
	config.hostmanager.manage_guest = true
	# The VirtualBox Guest Additions are not preinstalled; if you need them for shared folders,
	# please install the vagrant-vbguest (https://github.com/dotless-de/vagrant-vbguest) plugin and
	# add the following line to your Vagrantfile:
	config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
	# Please check the release blog for a full list of known limitations

	# The Vagrant Docker provisioner can automatically install Docker. However, the ask here is to install it via Ansible
	# config.vm.provision "docker"
	config.vm.define "techtest", primary: true do |techtest|
		techtest.vm.hostname = 'techtest'
		techtest.vm.network :private_network, ip: "192.168.99.201"
		techtest.vm.provider :virtualbox do |v|
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--memory", 3000]
			v.customize ["modifyvm", :id, "--name", "techtest"]
		end
		techtest.vm.provision "shell", name: "techtest_prerequisites", inline: "yum install -y epel-release && yum install -y git jq python-pip"
		techtest.vm.provision "shell", name: "molecule_prerequisites", inline: "yum install -y gcc python-devel openssl-devel libselinux-python"
		techtest.vm.provision "shell", name: "install_molecule", inline: "runuser -l  vagrant -c 'pip install --user molecule'"
		techtest.vm.provision "shell", name: "additional_tools", inline: "yum install -y net-tools"
		# https://www.vagrantup.com/docs/provisioning/ansible_local.html
		techtest.vm.provision "ansible_local" do |ansible|
			ansible.provisioning_path		=	"/vagrant/ansible/"
			ansible.playbook 						= "site.yml"
			ansible.inventory_path 			= "hosts"
			ansible.config_file					= "ansible.cfg"
			ansible.compatibility_mode	= "2.0"
			# ansible.verbose        			= true
		end
	end
end
