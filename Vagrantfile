# -*- mode: ruby -*-
# vi: set ft=ruby :

# Ubuntu 16.04 with Node 8.x, npm, git, couchbase, dockers
# By Somanath Bhat
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 18091, host: 18091
  config.vm.network "forwarded_port", guest: 8091, host: 8091
  
  for i in 3000..3010
    config.vm.network :forwarded_port, guest: i, host: i
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Customize the amount of memory and cpu on VM
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = "4"
    vb.memory = "6144"
    #vb.gui = true
  end
  
  config.vm.provision "node8", type: "shell", run:"once", inline: <<-SHELL
    apt-get clean
    apt-get update
    cd ~
    curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
    bash nodesource_setup.sh
    apt-get install -y nodejs
    apt-get install -y build-essential
	apt-get install -y python-httplib2
	

  SHELL
  
   # vagrant provision --provision-with docker  run:never|once|always
  config.vm.provision "docker", type: "shell", run:"once", inline: <<-SHELL
    echo "Provisioning docker ---------------------------------------------"
#	
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    echo " Expected fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88 "
    sudo apt-key fingerprint 0EBFCD88 | grep fingerprint
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get install -y docker-ce
    sudo docker run hello-world
    sudo systemctl enable docker	
    echo "Completed provisioning docker"
  SHELL
  
  # vagrant provision --provision-with couch 
  config.vm.provision "couch", type: "shell", run:"once", inline: <<-SHELL
    apt-get update
    cd ~
    wget https://packages.couchbase.com/releases/5.0.1/couchbase-server-community_5.0.1-ubuntu16.04_amd64.deb
dpkg -i couchbase-server-community_5.0.1-ubuntu16.04_amd64.deb
  SHELL
  
  
end
