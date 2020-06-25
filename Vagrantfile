# -*- mode: ruby -*-
# vi: set ft=ruby :

# if you wish to change any settings in here for yourself only, please see the
# instructions at the top of Vagrantfile.local.sample

BOX_NAME = "Digicel_VM"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = "Salmon/Centos-7"
	config.vm.box_version = "1.0.6"
    config.vm.host_name = "digicel.localvm"
	config.vm.graceful_halt_timeout = 120
	#config.vm.network "private_network", type: "dhcp"
	config.vm.network :private_network, ip: "172.28.128.7"
	
	config.vm.provider "virtualbox" do |v, override|
        v.memory = 4096
        v.cpus = 2
        v.name = BOX_NAME
    end
	
	config.vm.synced_folder ".", "/vagrant",
    type: "rsync",
	        rsync__exclude: [
            "/magento2/generated/[!.]*",
            "/magento2/pub/static/[!.]*",
            "/magento2/pub/media/[!.]*",
            "/magento2/var/[!.]*",
            "/magento2/vendor",
            "/magento2/dev/tests/acceptance/tests/functional/Magento/FunctionalTest/_generated",
            "/magento2/dev/tests/acceptance/tests/_output",
        ],
		        rsync__args: [
            "--verbose",
            "--archive",
            "--delete",
            "-z",
        ],
		owner: "vagrant",
        group: "vagrant",
        rsync__auto: true
		
	# gatling rsync is a less intensive rsync process which can be used instead
    # of rsync-auto, see https://github.com/smerrill/vagrant-gatling-rsync
    if Vagrant.has_plugin?("vagrant-gatling-rsync")
        config.gatling.latency = 1.5
        config.gatling.time_format = "%H:%M:%S"
        config.gatling.rsync_on_startup = true
    end
	
	config.vm.provision "ansible_local" do |ansible|
		ansible.become = true
		ansible.playbook = "/vagrant/ansible/playbook.yaml"
		ansible.verbose = "vvvv"
		ansible.extra_vars = "/vagrant/ansible/vars/dev_var.yaml"
		

   end 
end
