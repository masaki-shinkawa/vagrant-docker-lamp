# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.ssh.forward_agent = true
  config.vm.synced_folder ".", "/vagrant", mount_options: ['dmode=777','fmode=755']

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "2048"
    vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
  end

  config.vm.provision "shell", inline: <<-EOT
    apt update
    wget -qO- https://get.docker.com/ | sh
    docker build -t php:custom /vagrant/docker/php
    docker run --name mysql -v /vagrant/docker/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=pass -d mysql:5.7
    docker run -p 80:80 -v /vagrant/src:/var/www/html --link mysql:mysql --name php -d php:custom
  EOT
end

## phpのdockerに接続
# docker exec -ti php bash

## mysqlのdockerに接続
# docker exec -ti mysql bash