# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  # config.vm.box = "base"
  config.vm.box = "archlinux/archlinux"
  # config.vm.box = "terrywang/archlinux"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "devbox"
    vb.cpus = 4
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
    # Customize the amount of memory on the VM:
    vb.memory = "8192"
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
    vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    # install python as prequesite for ansible
    pacman -S archlinux-keyring
    pacman-key --populate

    # update mirrors
    curl -s "https://www.archlinux.org/mirrorlist/?country=DE&protocol=https&use_mirror_status=on" | sed -e 's/^#Server/Server/' -e '/^#/d' > /etc/pacman.d/mirrorlist

    # ugrade packages but ignore kernel since this may imply reboot
    pacman -Syyu --ignore linux
    # install python as perquisite for ansible
    pacman -S python --needed --noconfirm
  SHELL

  # Run Ansible from the Vagrant Host
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
      # ansible.verbose = "v"
      # ansible.start_at_task = "Display vars"
  end

end
