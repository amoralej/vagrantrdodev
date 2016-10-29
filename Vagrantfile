# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.hostname = "vmname.example.com"
  config.vm.box = "centos/7"
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.ssh.forward_agent = true
  
  config.vm.provider "libvirt" do |libvirt|
    libvirt.driver = "kvm"
    libvirt.host = "localhost"
    libvirt.connect_via_ssh = true
    libvirt.username = "root"
    libvirt.storage_pool_name = "default"
    libvirt.memory = 6144
    libvirt.cpus = 3
  end

  config.vm.provider "openstack" do |os, override|
    override.ssh.username = "centos"
    override.ssh.private_key_path = "~/.ssh/id_dsa"
    override.ssh.pty = true

    # Specify OpenStack authentication information
    os.openstack_auth_url = ENV['OS_AUTH_URL'] + "tokens"
    os.username = ENV['OS_USERNAME']
    os.password = ENV['OS_PASSWORD']
    os.tenant_name = ENV['OS_TENANT_NAME']

    # Specify instance information
    os.server_name = "vagrant-test"
    os.flavor = "m1.large"
    os.image = "centos-7-cloud"
    os.floating_ip_pool = "external"
    os.networks = ["default-network-amoralej-ff834"]
    os.keypair_name = "amoralej"
  end

  config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"
  config.vm.provision "file", source: "~/.vimrc", destination: ".vimrc"

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = true
    ansible.playbook = "playbook.yml"
  end

end
