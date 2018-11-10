# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 2.1"

ANSIBLE_VERSION = "2.4.*"

Vagrant.configure("2") do |config|

  config.vm.box = "bento/ubuntu-18.04"

  config.vm.synced_folder "./", "/usr/local/src"
  config.vm.synced_folder "~/.password-store", "/home/vagrant/.password-store"
  config.vm.synced_folder "~/.gnupg", "/home/vagrant/.gnupg"
  config.vm.synced_folder "~/.aws", "/home/vagrant/.aws"

  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "goobzie"
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.install = true
    ansible.install_mode = "pip_args_only"
    ansible.pip_args = "ansible==#{ANSIBLE_VERSION}"
    ansible.playbook = "provision/ansible/dev.yml"
    ansible.galaxy_role_file = "provision/ansible/roles.yml"
    ansible.galaxy_roles_path = "provision/ansible/roles"
  end

  config.vm.provision "shell" do |s|
    s.inline = <<-SHELL
    if ! grep -q "cd /usr/local/src" "/home/vagrant/.bashrc"; then
      echo "cd /usr/local/src" >> "/home/vagrant/.bashrc"
    fi
    SHELL
  end

end
