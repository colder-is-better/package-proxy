# -*- mode: ruby -*-
# vi: set ft=ruby :

require "yaml"
p = YAML.load_file "machine_vars.yml"

Vagrant.configure(2) do |config|

# ===== General Vagrant VM configuration

  config.vm.box = "sub0/debian950srv"
  config.vm.box_check_update = true
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.ssh.insert_key = false
  config.vm.provider :virtualbox do |tmpl|
    tmpl.memory = p["ram_size"]
    tmpl.cpus = p["core_count"]
    tmpl.linked_clone = true
  end

# ===== Proxy node

  config.vm.define "theproxy" do |node|
    node.vm.hostname = p["fqdn"]
    node.vm.network :private_network, ip: p["ipaddr"]
  end

# ===== Ansible provisioner

  config.vm.provision :ansible do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "provision.yml"
    ansible.become = true
  end
end
