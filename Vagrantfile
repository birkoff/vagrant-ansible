# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chef/ubuntu-14.04"

  config.vm.define "blueberryh_dev" do |blueberryh_dev|
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  
  config.vm.network "private_network", ip: "10.250.250.251"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
    
  config.ssh.forward_agent = true
  config.ssh.forward_x11   = true
  
  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  
  [
    { :host => './',              :guest => '/vagrant' },
    { :host => '~/.ssh',          :guest => '/.ssh' },
    { :host => '~/workspace/doctrine',     :guest => '/var/www/html/doctrine' },
  ].each do |folder|
    config.vm.synced_folder folder[:host], folder[:guest], type: 'nfs'
  end

  # Ansible provisioning
  config.vm.provision "ansible" do |ansible|
    # Perform sudo commands
    ansible.sudo = true
    ansible.ask_sudo_pass = false

    # Location of the playbook
    ansible.playbook = "provisioning/playbook.yml"

    # Specify which groups the machine is in
    ansible.groups = {
      'webserver' => ['blueberryh_dev'],
      'mysqlserver' => ['blueberryh_dev'],
      'all_groups:children' => ['webserver', 'mysqlserver']
    }

    # Some extra configuration
    ansible.extra_vars = {
      ansible_ssh_user: 'vagrant',
      host_username: `whoami`
    }
  end

end
