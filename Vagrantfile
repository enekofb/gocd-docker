# -*- mode: ruby -*-
# vi: set ft=ruby :

# Require YAML module
require 'yaml'

# Read YAML file with box details
cfg = YAML.load_file('config.yml')
DEVELOPMENT_HOST=cfg['DEVELOPMENT_HOST']

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  #config.vm.hostname = 'go'
  config.vm.box = "ubuntu/trusty64"

  #config.vm.network :forwarded_port, guest: 8153, host: 8153

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  #config.vm.hostname = "odilott-gateway"
  config.vm.network :private_network, ip: DEVELOPMENT_HOST

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provision :docker do |docker|
    docker.run "gocd/gocd-server", args: "-i -t -p 8153:8153 --name=gocd-server"
    docker.run "gocd/gocd-agent", args: "-i -t -p 8152:8152 --name=gocd-agent --link gocd-server:go-server"
  end

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", 2048]
    vb.customize ["modifyvm", :id, "--cpus", 2]
  end

end
