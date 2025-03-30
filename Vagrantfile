# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64" # define a imagem que será utilizada

  # servidor web
  config.vm.define "web" do |web|
    web.vm.hostname = "servidor-web"
    web.vm.network "forwarded_port", guest: 80, host: 8090

    web.vm.provision "shell", inline: <<-WEB
      apt update
      apt install apache2 -y
      systemctl restart apache2
    WEB
  end

  # banco de dados
  config.vm.define "db" do |db|
    db.vm.hostname = "database"
    db.vm.network "private_network", type: "static", ip: "192.168.50.10" 
    db.vm.provision "shell", inline: <<-DB
      apt update
      apt install mariadb-server -y
      systemctl restart mariadb.service
    DB
  end

end
