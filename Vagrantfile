Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
  end

  [ "ns01", "ns02", "client", "client2" ].each do |hostname|
    config.vm.define hostname do |vm|
      ip = case hostname
        when "ns01" then "192.168.50.10"
        when "ns02" then "192.168.50.11"
        when "client" then "192.168.50.15"
        when "client2" then "192.168.50.16"
      end

      vm.vm.network "private_network", ip: ip
      vm.vm.hostname = hostname

      vm.vm.provision "shell", inline: <<-SHELL
        # Разрешить Password и Pubkey Auth
        sudo sed -i 's/#*PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config
        sudo sed -i 's/#*PubkeyAuthentication.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config
        sudo sed -i 's|^Include /etc/ssh/sshd_config.d/\\*.conf|#Include /etc/ssh/sshd_config.d/*.conf|' /etc/ssh/sshd_config
        sudo systemctl restart sshd
      SHELL

      # Только для ns01 — установка Ansible и запуск playbook
      if hostname == "ns01"
        vm.vm.provision "shell", inline: <<-SHELL
          sudo apt-get update
          sudo apt-get install -y software-properties-common
          sudo add-apt-repository --yes --update ppa:ansible/ansible
          sudo apt-get install -y ansible

          cd /vagrant/provisioning
          ansible-playbook -i "localhost," -c local playbook.yml
        SHELL
      end
    end
  end
end
