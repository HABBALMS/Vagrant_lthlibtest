# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    
  # Common configuration for both machines.
  config.vm.box = "ubuntu/jammy64"
  config.ssh.insert_key = false
  config.vm.provision "file", source: "keys/config",  destination: "~/.ssh/config"
  config.vm.provision "file", source: "keys/vagrant", destination: "~/.ssh/id_rsa"
  config.vm.provision "file", source: "keys/vagrant.pub", destination: "~/.ssh/id_rsa.pub"
  config.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/id_rsa"
  config.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/id_rsa.pub"
  config.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/config"

  # Define the first node.
  config.vm.define "lthlibsftp" do |lthlibsftp|
    lthlibsftp.vm.hostname = "lthlibsftp"
    lthlibsftp.vm.provider :virtualbox do |vb|
        vb.memory = 3072
        vb.cpus = 2
    end
    lthlibsftp.vm.network "private_network", ip: "192.168.56.11"
  end

  # Define the second node.
  config.vm.define "lthlibprod" do |lthlibprod|
    lthlibprod.vm.hostname = "lthlibprod"
    lthlibprod.vm.provider :virtualbox do |vb|
        vb.memory = 3072
        vb.cpus = 2
    end
    lthlibprod.vm.network "private_network", ip: "192.168.56.12"
  end

# Define the master node.
  config.vm.define "master", primary: true do |master|
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "192.168.56.10"
    # master.vm.network "forwarded_port", guest: 9090, host: 9090
    master.vm.provider :virtualbox do |vb|
        vb.memory = 3072
        vb.cpus = 2
    end
    master.vm.provision :ansible_local do |ansible|
      ansible.verbose        = true
      ansible.install        = true
      ansible.limit          = "all"
      ansible.inventory_path = "inventory"
      ansible.playbook       = "lib_vms.yml"
    end
  end
end
