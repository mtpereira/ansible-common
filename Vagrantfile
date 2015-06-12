# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

vmname = File.basename(File.expand_path(File.dirname(__FILE__)))

Vagrant.configure("2") do |config|
  config.vm.define vmname do |ap|
    ap.vm.box = "chef/debian-7.8"
    ap.vm.hostname = vmname

    ap.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["createhd", "--filename", "common_filesystems_disk.vdi", "--size", 50]
      v.customize ["storageattach", :id, "--storagectl", "IDE Controller", "--port", 1, "--device", 0, "--type", "hdd", "--medium", "common_filesystems_disk.vdi"]
    end

    ap.vm.network :private_network, ip: "10.0.0.40"

    ap.vm.provision :ansible do |ansible|
      ansible.playbook = "test.yml"
      ansible.verbose = ENV['ANSIBLE_VERBOSE'] ||= "vv"
    end
  end
end
