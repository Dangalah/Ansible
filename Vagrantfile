# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'etc'

def form_ips(is_home = "false", num = 0)
    @is_home = is_home.to_s.downcase == "true"
    @header = "10.211."
    if @is_home
        return "%s100.%d" % [@header, 100 + num]
    else
        @suffix = Etc.getlogin[-3..-1].to_i * 3
        return "%s%d.%d" % [@header, @suffix / 256, @suffix % 256 + num + 10]
    end
end

@ip1 = form_ips ENV['IS_HOME']
puts "This script will start these VMs:"
puts "%s-node1 with IP %s" % [Etc.getlogin, @ip1]

$update_ubuntu = <<SCRIPT
sudo apt update
sudo apt-get install python-dev python3-dev -y
SCRIPT

Vagrant.configure("2") do |config|
  config.hostmanager.enabled = false
  config.hostmanager.manage_guest = true
  config.hostmanager.include_offline = true
  config.hostmanager.ignore_private_ip = false
  config.ssh.forward_agent = true
  config.vm.synced_folder '.', '/vagrant', disabled: true


  config.vm.define :node1 do |node1|
    node1.vm.box = "ubuntu/bionic64"
    node1.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "1024"
    end
    node1.vm.network :private_network, ip: @ip1
    node1.vm.hostname = Etc.getlogin + "-node1"
    node1.vm.provision :hostmanager
    node1.vm.provision :shell, :inline => $update_ubuntu
    node1.vm.provision :shell, inline: <<-SHELL
      sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      sudo service ssh restart
    SHELL
  end
end

