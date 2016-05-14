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
  sudo apt-get -y install libffi-dev libssl-dev # 05/10/16 failed without these for some reason
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get -y install python-dev
  sudo apt-get -y install python-pip
  sudo pip install pip --upgrade
  sudo pip install --upgrade setuptools # 05/10/16 failed without this addition
  sudo pip install ansible
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
  config.vm.network "forwarded_port", guest: 5432, host: 5432 # postgis
  config.vm.network "forwarded_port", guest: 5555, host: 5555 # flower
  config.vm.network "forwarded_port", guest: 80, host: 80 # nginx default
  config.vm.network "forwarded_port", guest: 8001, host: 8001 # osmexport
  config.vm.network "forwarded_port", guest: 25, host: 25 # postfix

  # the root directory ( the directory with Vagrantfile ) shared by default to:
  # /usr/local/src/<root> => /vagrant/<root>
  # /usr/local/src/<root>/<playbookdir> => /usr/local/src/<root>/<playbookdir>

  # example of sharing data directory
  config.vm.synced_folder "dev", "/usr/local/src/dev",
                          id: "dev",
                          owner: "vagrant",
                          group: "vagrant",
                          mount_options: ["dmode=777","fmode=777"],
                          create: false

  #
  # provision with Ansible
  #
  config.vm.provision "shell", inline: $ansible_install_script # make sure ansible is installed on guest
  config.vm.provision "ansibleLocal" do |ansible|
    db_password = ENV['DB_PASSWORD']
    if db_password.nil?
        raise StandardError, "You must set an environment variable called 'DB_PASSWORD' before calling `vagrant up`" 
    end
    ansible.extra_vars = { "DB_PASSWORD" => db_password }
    ansible.guest_folder = CWD + "/ops"
    ansible.raw_arguments = "--inventory="+CWD+"/ops/inventories/vagrant.ini"
    ansible.limit = "all"
    ansible.playbook = CWD+"/ops/provision_osm_export_tool.yml"
  end

end

