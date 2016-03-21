# -*- mode: ruby -*-
# vi: set ft=ruby :

#
# this Vagrantfile requires additional plugins
# to work for running Ansible inside the VM
# execute on your host once:
#
# vagrant plugin install vagrant-vbguest
# vagrant plugin install vagrant-ansible-local
#
CWD = Dir.pwd
VAGRANTFILE_API_VERSION = "2"

#
# ansible install script for ubuntu
#
$ansible_install_script = <<SCRIPT
if ! which ansible >/dev/null; then
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get -y install python-dev
  sudo apt-get -y install python-pip
  sudo pip install pip --upgrade
  sudo pip install ansible
  sudo pip install ansible-vault
fi
SCRIPT

# check if vagrant-ansible-local plugin is installed
if !Vagrant.has_plugin?('vagrant-ansible-local')
    puts "The vagrant-ansible-local plugin is missing!"
    puts "Install the plugin with this command and rerun Vagrant:"
    puts
    puts "    vagrant plugin install vagrant-ansible-local"
    puts
    exit 1
end

Vagrant.configure(2) do |config|
  config.vm.box = "trusty64-osmexport-03202016"
  config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.vm.hostname = 'trusty64-osmexport-03202016'

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2024"
  end

  #
  # port forwarding
  #
  config.vm.network "forwarded_port", guest: 5432, host: 5432 
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 3001, host: 3001

  # the root directory ( the directory with Vagrantfile ) shared by default to:
  # /usr/local/src/<root> => /vagrant/<root>
  # /usr/local/src/<root>/<playbookdir> => /usr/local/src/<root>/<playbookdir>


  # example of sharing data directory 
  # config.vm.synced_folder "./data", "/usr/local/src/data",
  #                        id: "ops",
  #                        owner: "vagrant",
  #                        group: "vagrant",
  #                        mount_options: ["dmode=775","fmode=664"],
  #                        create: true

  #
  # provision with Ansible
  #
  config.vm.provision "shell", inline: $ansible_install_script # make sure ansible is installed on guest
  config.vm.provision "ansibleLocal" do |ansible|
    ansible.guest_folder = CWD + "/ops"
    ansible.raw_arguments = "--inventory="+CWD+"/ops/inventories/vagrant.ini"
    ansible.limit = "all"
    ansible.playbook = CWD+"/ops/provision_osm_export_tool.yml"
  end

  #
  # share a folder but don't mount it
  # until Ansible has run it's provisioning
  #

end

