# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  N = 3

  # PUBLIC_KEY_FILE = File.read(".ssh/id_rsa.pub")
  (1..N).each do |machine_id|
    name = "vagrant-hashi-stack-#{machine_id}"
    # config.vm.provision "file", source: ".ssh/id_rsa", destination: "$HOME/.ssh/id_rsa"
    # config.vm.provision "file", source: ".ssh/id_rsa.pub", destination: "$HOME/.ssh/id_rsa.pub"
    
    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end

    config.vm.define name do |machine|
    #   machine.vm.customize ["modifyvm", :id, "--memory", 1024]
      machine.vm.box = "debian/stretch64"
      machine.vm.hostname = "#{name}.alpha.consul"
      machine.vm.network "private_network", ip: "192.168.33.#{10+machine_id-1}"
      # machine.vm.provision "file", source: ".ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"

      if name == "vagrant-hashi-stack-1"
        machine.vm.synced_folder "nomad/", "/home/vagrant/workspace/nomad"
        machine.vm.synced_folder "vault/", "/home/vagrant/workspace/vault"
        machine.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
        end
      end
      #else
      # do nothing for now
      #end

        #  echo '#{PUBLIC_KEY_FILE}' >> /home/vagrant/.ssh/authorized_keys
        # chmod -R 600 /home/vagrant/.ssh/authorized_keys
        # echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
        # echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
        # echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
        # chmod -R 600 /home/vagrant/.ssh/config

          # curl -s -L -o /home/vagrant/.vimrc https://raw.githubusercontent.com/amix/vimrc/master/vimrcs/basic.vim

      machine.vm.provision "shell", privileged: false, inline: <<-SHELL
        echo "==> Shell Provisioning"

        VAGRANT_SSH_DIRECTORY="/home/vagrant/.ssh"
        if [ ! -d "$VAGRANT_SSH_DIRECTORY" ]; then
          mkdir /home/vagrant/.ssh
        fi

        sudo apt-get update -y && sudo apt-get upgrade -y \
          && sudo apt-get -y install vim wget git zip unzip tree curl build-essentials 

        file="/home/vagrant/.vimrc"
        if [ -f "$file" ]
        then
            echo ".vimrc exists, will not download again."
        else
          wget -q -O /home/vagrant/.vimrc https://raw.githubusercontent.com/amix/vimrc/master/vimrcs/basic.vim
        fi

      SHELL

    end
  end
end
