# Defines our Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'rackspace'

  # create some web servers
  # https://docs.vagrantup.com/v2/vagrantfile/tips.html
  (1..1).each do |i|
    config.vm.define "mgnt" do |node|
	node.ssh.proxy_command = "ssh -aY cbast.lon3.rackspace.com -l jose7890 \"nc -w 900 %h %p\""
	node.ssh.private_key_path = "~/.ssh/id_jgasparcloud"
	node.vm.hostname = "mgnt.mywebsiteinthecloud.com"
	#node.vm.synced_folder ".", "/vagrant", disabled: true
	node.vm.synced_folder "/opt/mgnt/", "/vagrant/", create: true
	node.vm.boot_timeout = 10
      	node.vm.provider "rackspace" do |rs|
    	  rs.username        = <USERNAME> 
    	  rs.api_key         = <USERKEY> 
    	  rs.rackspace_region= "LON"
    	  rs.flavor          = /4 GB General Purpose v1/
    	  rs.image           = /Ubuntu 15.10/
    	  rs.server_name     = "mgnt"
    	  #rs.public_key_path = "~/.ssh/id_jgasparcloud.pub"
	  rs.key_name	  = "SysOps"
        end

	node.vm.provision :ansible do |as|
	  as.playbook = "/home/jgaspar/sysops/mgnt.yml"
	  as.raw_ssh_args = "-o ProxyCommand=\"ssh -aY cbast.lon3.rackspace.com -l jose7890 'nc -w 900 %h %p'\""
	end

        node.vm.provision :shell, path: "bootstrap-mgmt.sh"
    end
  end

end
