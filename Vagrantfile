boxes = [
  {
    :name      => File.basename(__dir__) + '.debian-wheezy',
    :box       => 'boxcutter/debian78',
    :ip        => '10.0.0.40',
    :cpu_cap   => '50',
    :cpus      => 1,
    :ram       => '512',
    :disk_path => __dir__ + '/.vagrant/common_filesystems_disk.vdi'
  },
]

Vagrant.configure("2") do |config|
  boxes.each do |box|
    config.vm.define box[:name] do |vms|
      vms.vm.box = box[:box]
      vms.vm.hostname = "%s" % box[:name]
      vms.vm.network :private_network, ip: box[:ip]
      vms.vm.provider "virtualbox" do |v|
        v.memory = box[:ram]
        v.cpus = box[:cpus]
        v.customize ["modifyvm", :id, "--cpuexecutioncap", box[:cpu_cap]]
        unless File.exists?(box[:disk_path])
				  v.customize ["createhd", "--filename", box[:disk_path], "--size", 128]
        end
        v.customize ["storageattach", :id, "--storagectl", "IDE Controller",
                     "--port", 1, "--device", 0, "--type", "hdd", "--medium",
                     box[:disk_path]]
      end
      vms.vm.provision :ansible do |ansible|
        ansible.playbook = "test.yml"
        ansible.host_key_checking = false
        ansible.verbose = ENV['ANSIBLE_VERBOSE'] ||= "vv"
        ansible.tags = ENV['ANSIBLE_TAGS'] ||= "all"
      end
    end
  end
end

# -*- mode: ruby -*-
# vim: set ft=ruby ts=2 sw=2 tw=0 et :

