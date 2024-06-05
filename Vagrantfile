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

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/clone_roles.yml"
    ansible.extra_vars = {
      git_repository: "https://github.com/EvilSeeQu-sys/Ansible_roles",
      git_branch: "main"
    }
  end

  # Provisions with ansible
  config.vm.provision "ansible_local" do |ansible|
    ansible.galaxy_role_file = 'requirements.yml'
    ansible.galaxy_roles_path = "/etc/ansible/roles"
    ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
    ansible.playbook = "playbooks/init.yml"
#    ansible.verbose = "vvv"
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/infrastructure.yml"
    ansible.extra_vars = {
      git_repository: "https://github.com/EvilSeeQu-sys/Infrastructure",
      git_branch: "main"
    }
  end

  if VAGRANT_COMMAND == "ssh"
    config.ssh.username = 'panda'
  end
end
