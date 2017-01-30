# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'etc'
require 'shellwords'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "bento/centos-7.3"
  config.vm.hostname = "devel"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", type: "dhcp"

  # Tweak the VMs configuration.
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = Etc.nprocessors
    vb.memory = 2048
    vb.linked_clone = true
  end

  # configure ssh to pass through certain AWS environment variables for auth
  config.ssh.forward_env = ["AWS_ACCESS_KEY_ID", "AWS_SECRET_ACCESS_KEY"]

  # Configure the VM using Ansible
  config.vm.provision "ansible_local" do |ansible|
    # allow passing ansible args from environment variable
    ansible.raw_arguments = Shellwords::shellwords(ENV.fetch("ANSIBLE_ARGS", ""))

    ansible.provisioning_path = "/vagrant/ansible/"
    ansible.playbook = "playbook.yml"

    # ansible.galaxy_role_file = "requirements.yml"
    # ansible.galaxy_roles_path = "galaxy_roles"
  end
end
