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

	# The Vagrant Docker provisioner can automatically install Docker. However, the ask is to install it via Ansible
	# config.vm.provision "docker"
	config.vm.define "techtest", primary: true do |techtest|
		techtest.vm.hostname = 'techtest'
		techtest.vm.network :private_network, ip: "192.168.99.201"
		techtest.vm.provider :virtualbox do |v|
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--memory", 3000]
			v.customize ["modifyvm", :id, "--name", "techtest"]
		end
		# techtest.vm.provision "shell", inline: "yum install -y epel-release && yum install -y git jq wget"
		techtest.vm.provision "ansible_local" do |ansible|
			ansible.playbook = "ansible/site.yml"
		end
	end
end