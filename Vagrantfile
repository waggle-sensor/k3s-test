# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  # config.vm.network "private_network", ip: "10.31.81.200"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SCRIPT
    wget -q https://github.com/k3s-io/k3s/releases/download/v1.19.7%2Bk3s1/k3s -O /usr/local/bin/k3s
    chmod +x /usr/local/bin/k3s

    cat <<EOF > /usr/local/bin/kubectl
#!/bin/bash
/usr/local/bin/k3s kubectl \\$*
EOF
  chmod +x /usr/local/bin/kubectl
  SCRIPT
   
end
