# -*- mode: ruby -*-
# vi: set ft=ruby :

require "yaml"
params = YAML.load_file "setup_vars.yml"

Vagrant.configure(2) do |config|

# ===== General Vagrant VM configuration

  config.vm.box = "sub0/debian950srv"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :virtualbox do |tmpl|
    tmpl.memory = params["ram_size"]
    tmpl.cpus = params["core_count"]
    tmpl.linked_clone = true
  end

# ===== Proxy node

  config.vm.define "theproxy" do |node|
    node.vm.hostname = params["proxy_fqdn"]
    node.vm.network :private_network, ip: params["proxy_ipv4"]
  end

# ===== Ansible provisioner

  config.vm.provision :ansible do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "provision.yml"
    ansible.become = true
  end
end
