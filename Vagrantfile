# -*- mode: ruby -*-
# vi: set ft=ruby :

# Require YAML module
require 'yaml'
# Read YAML file with box details
vars = YAML.load_file('vars.yml')

Vagrant.configure("2") do |config|
  config.vm.box = "archlinux/archlinux"

  if Vagrant.has_plugin?("vagrant-proxyconf")
    puts "using proxy"
    config.proxy.enabled = true
    config.proxy.http = "http://" + vars['proxy']['address']
    config.proxy.https = "http://" + vars['proxy']['address']
    config.proxy.no_proxy = vars['proxy']['noproxy']
  end

  config.vm.provider "virtualbox" do |vb|
    vb.name = "devbox"
    vb.cpus = 4
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
    # Customize the amount of memory on the VM:
    vb.memory = "16384"
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
    vb.customize ["modifyvm", :id, "--monitorcount", "2"]
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
    vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    # update pacman keys
    pacman -S archlinux-keyring
    pacman-key --populate
  SHELL

  # Run Ansible from the Vagrant Host
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    # ansible.verbose = "vv"
    # ansible.start_at_task = "Update packages"
  end

end
