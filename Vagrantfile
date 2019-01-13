# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
	# https://app.vagrantup.com/ralfkrause/boxes/centos7
	config.vm.box = "ralfkrause/centos7"
	config.vm.box_version = "1.0.0"
	config.hostmanager.enabled = true
	config.hostmanager.manage_host = true
	config.hostmanager.manage_guest = true

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
