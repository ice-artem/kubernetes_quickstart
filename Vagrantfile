# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos/7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", type: "dhcp"  #ip: "192.168.133.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
 # Customize the amount of memory on the VM:
vb.memory = "512"
end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL

  config.vm.define "master" do |master|
      master.vm.hostname = "master"
      master.vm.network "private_network", ip: "192.168.50.130"
#      master.vm.network "private_network", ip: "192.168.20.130"
#      config.vm.provider :virtualbox do |vb|
#        vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
#      end
  end

  config.vm.define "minion_1" do |minion_1|
      minion_1.vm.hostname = "minion-1"
      minion_1.vm.network "private_network", ip: "192.168.50.131"
#      minion_1.vm.network "private_network", ip: "192.168.20.131"
#      config.vm.provider :virtualbox do |vb|
#        vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
#      end
  end

  config.vm.define "minion_2" do |minion_2|
      minion_2.vm.hostname = "minion-2"
      minion_2.vm.network "private_network", ip: "192.168.50.132"
#      minion_2.vm.network "private_network", ip: "192.168.20.132"
#      config.vm.provider :virtualbox do |vb|
#        vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
#      end
  end

  config.vm.define "ingress" do |ingress|
      ingress.vm.hostname = "ingress"
      ingress.vm.network "private_network", ip: "192.168.50.133"
      ingress.vm.network "private_network", ip: "192.168.11.11", auto_config: false
      ingress.vm.provision "shell", inline: <<-SHELL
                        echo "NM_CONTROLLED=no\nBOOTPROTO=none\nONBOOT=yes\nIPADDR=192.168.11.11\nIPADDR1=192.168.11.12\nIPADDR2=192.168.11.13\nIPADDR3=192.168.11.14\nIPADDR4=192.168.11.15\nIPADDR5=192.168.11.16\nNETMASK=255.255.255.0\nDEVICE=eth2\nPEERDNS=no" > /etc/sysconfig/network-scripts/ifcfg-eth2
                        systemctl restart network
		SHELL
  end

  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/me.pub"

#  config.vm.provision "shell", path: "provision.sh"
  
  config.vm.provision "shell", inline: <<-SHELL
  cat /home/vagrant/.ssh/me.pub >> /home/vagrant/.ssh/authorized_keys
  sudo mkdir /root/.ssh
  sudo cp /home/vagrant/.ssh/authorized_keys /root/.ssh
  SHELL

end
