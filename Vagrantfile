VAGRANT_COMMAND = ARGV[0]

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"

  config.vm.provider "virtualbox" do |v|
    v.memory = 16240
    v.cpus = 6
  end

  config.vm.network "private_network", ip: "192.168.56.115"

  # Synchronizacja folderu z kluczem SSH
  config.vm.synced_folder "/home/evilseequ/bigspace/vagrant/", "/vagrant/PandaCore"

  # Provisions with ansible
  config.vm.provision "ansible_local" do |ansible|
    ansible.galaxy_role_file = 'requirements.yml'
    ansible.galaxy_roles_path = "/etc/ansible/roles"
    ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
    ansible.playbook = "playbooks/init.yml"
  end

  # Shell script to configure ssh key and ssh config for user panda
  config.vm.provision "shell", inline: <<-SHELL
    mkdir -p /home/panda/.ssh
    cp /vagrant/PandaCore/panda-ssh /home/panda/.ssh/id_rsa_git
    chown panda:panda /home/panda/.ssh/id_rsa_git
    chmod 600 /home/panda/.ssh/id_rsa_git

    # Create and configure ssh config to use the specific key for git operations
    echo "Host github.com" > /home/panda/.ssh/config
    echo "  IdentityFile /home/panda/.ssh/id_rsa_git" >> /home/panda/.ssh/config
    echo "  StrictHostKeyChecking no" >> /home/panda/.ssh/config
    chown panda:panda /home/panda/.ssh/config
    chmod 644 /home/panda/.ssh/config
  SHELL

  if VAGRANT_COMMAND == "ssh"
    config.ssh.username = 'panda'
  end
end
