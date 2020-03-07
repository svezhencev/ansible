
require 'getoptlong'

VAGRANTFILE_API_VERSION = "2"


opts = GetoptLong.new(
  [ '--playbook', GetoptLong::OPTIONAL_ARGUMENT ]
)

playbook='main'

opts.each do |opt, arg|
  case opt
    when '--playbook'
      playbook=arg
  end
end


Vagrant.configure(2) do |config|

  config.vm.box = "centos/7"

  config.vm.network :private_network, ip: "192.168.3.10"
  config.vm.hostname = "project.local"

  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false

  config.vm.synced_folder ".", "/vagrant", type: "rsync",
    rsync__exclude: [".git/", ".venv/", ".vagrant/", "vendor/", "roles/", "tmp/"]

  config.vm.provision "ansible" do |ansible|
    ansible.become = true
    ansible.limit = "all"
    ansible.verbose = "v"
    ansible.playbook = "playbooks/#{playbook}.yml"
  end
end
