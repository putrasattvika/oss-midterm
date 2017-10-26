require 'yaml'

Vagrant.configure("2") do |config|

  # Config file
  _conf = YAML.load(
    File.open(
      File.join(File.dirname(__FILE__), 'provision/default.yml'),
      File::RDONLY
    ).read
  )

  # Networking
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = _conf['hostname']
  config.vm.network :private_network, ip: _conf['ip']

  # Synchronized directories
  config.vm.synced_folder _conf['synced_folder'], _conf['phabricator_plugin_folder'],
      :create => "true",
      :mount_options => ['dmode=755', 'fmode=644']

  # Provision
  config.vm.provision "ansible_local" do |ansible|
    ansible.extra_vars = {
      vagrant: _conf
    }
    ansible.playbook = "provision/phabricator.yml"
  end
end