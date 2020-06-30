# -*- mode: ruby -*-
# vim: set ft=ruby :
home = ENV['HOME']
ENV["LC_ALL"] = "en_US.UTF-8"

Vagrant.configure("2") do |config|
  config.vm.box = "Centos/7"
  config.vm.box_version = "1804.02"
  config.vbguest.no_install = true
  config.vm.synced_folder ".", "/vagrant", disabled: true
  N = 2
  (1..N).each do |machine_id|
    config.vm.define "nginx#{machine_id}" do |machine|
      machine.vm.host_name = "nginx#{machine_id}"
      #machine.vm.network "forwarded_port", guest: 8080, host: 8080
      machine.vm.network "private_network", ip: "192.168.11.#{100+machine_id}"
      machine.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        #vb.gui = true
      end
      if machine_id == N
        machine.vm.provision "ansible" do |ansible|
          ansible.limit = "all"
          ansible.playbook = "playbook.yml"
        end
        machine.vm.provision "shell", inline: <<-SHELL, privileged: false
          echo -e "\n\nДомашнее задание Ansible-1\n"
          echo -e "\nПроверим запущенный nginx:\n"
          systemctl status nginx | head -n3
          echo -e "\n\nПроверим загрузку странички с порта 8080:\n"
          curl 127.0.0.1:8080 | head -n5
          echo -e "\n\nЗадание выполнено\n"
          echo "Спасибо за проверку!"
        SHELL
      end
    end
  end
end
