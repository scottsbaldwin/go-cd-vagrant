# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # config.ssh.forward_agent = true
  # config.vm.synced_folder "../data", "/vagrant_data"
  # cfg.vm.provider "virtualbox" do |vb|
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #

  server_ip        = "192.168.33.11"
  number_of_agents = 2

  config.vm.define "server" do |cfg|
    cfg.vm.box = "ubuntu/trusty64"
    cfg.ssh.forward_agent = true
    cfg.vm.synced_folder "~/Downloads/go", "/downloads"

    cfg.vm.network "forwarded_port", guest: 8153, host: 8153
    cfg.vm.network "forwarded_port", guest: 8154, host: 8154
    cfg.vm.network "private_network", ip: server_ip

    cfg.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
    end

    cfg.vm.provision "ansible" do |ansible|
      ansible.verbose = "vv"
      ansible.playbook = "ansible/roles/go-server/tasks/main.yml"
    end
  end

  (1..number_of_agents).each do |i|
    agent_ip = 20 + i
    config.vm.define "agent#{i}" do |cfg|
      cfg.vm.box = "ubuntu/trusty64"
      cfg.ssh.forward_agent = true
      cfg.vm.synced_folder "~/Downloads/go", "/downloads"

      cfg.vm.network "private_network", ip: "192.168.33.#{agent_ip}"

      cfg.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", "512"]
      end

      cfg.vm.provision "ansible" do |ansible|
        ansible.verbose = "vv"
        ansible.extra_vars = {
          go_server_ip: server_ip
        }
        ansible.playbook = "ansible/roles/go-agent/tasks/main.yml"
      end
    end
  end
end
