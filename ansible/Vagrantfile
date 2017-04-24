# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  $www_host = "www.supportbot.dev"
  $www_ip = "192.168.2.5"
  $db_host = "db.supportbot.dev"
  $db_ip = "192.168.2.6"

  # Base VM OS configuration.
  config.vm.box = "debian/jessie64"
  config.ssh.insert_key = false
  config.vm.synced_folder '.', '/vagrant', disabled: true

  # General VirtualBox VM configuration.
  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 1
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Nginx.
  config.vm.define "www" do |www|
    www.vm.hostname = $www_host
    www.vm.network :public_network, ip: $www_ip

    www.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 256]
    end
  end

  # MySQL.
  config.vm.define "db" do |db|
    db.vm.hostname = $db_host
    db.vm.network :public_network, ip: $db_ip

    # Run Ansible provisioner once for all VMs at the end.
    db.vm.provision "ansible" do |ansible|
      ansible.playbook = "configure.yml"
      ansible.inventory_path = "inventories/vagrant/inventory"
      ansible.limit = "all"
      ansible.extra_vars = {
        ansible_ssh_user: 'vagrant',
        ansible_ssh_private_key_file: "~/.vagrant.d/insecure_private_key"
      }
    end
  end
end
