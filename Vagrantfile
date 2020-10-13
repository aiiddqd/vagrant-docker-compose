unless Vagrant::DEFAULT_SERVER_URL.frozen?
    Vagrant::DEFAULT_SERVER_URL.replace('https://vagrantcloud.com')
  end

  Vagrant.configure("2") do |config|

    config.vm.synced_folder "./app/", "/srv/app/"

    if !Vagrant.has_plugin?("vagrant-docker-compose")
      print "  WARN: Missing plugin 'vagrant-docker-compose'.\n"
      print "  Use 'vagrant plugin install vagrant-docker-compose' to install.\n"
    end

    config.vm.network "private_network", ip: "192.168.33.33"
    config.vm.box = "ubuntu/xenial64"
    config.vm.provider "virtualbox" do |v|
      v.gui  = false
      v.name = "local-server"
      v.memory = 4024
    end

    config.vm.provision :docker_compose
    config.vm.provision :shell, inline: <<-SHELL2
      cd /srv/app/
      docker-compose up
    SHELL2

    config.ssh.forward_agent = true
  end
