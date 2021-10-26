# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|


  config.vm.define :mgmt do |mgmt_conf|
      mgmt_conf.vm.box = "generic/centos8"
      mgmt_conf.vm.hostname = "mgmt"
      mgmt_conf.vm.network :private_network, ip: "192.168.180.10"
      mgmt_conf.vm.provider "vmware_workstation" do |vb|
        vb.vmx["memsize"] = 1024
        vb.vmx["ethernet1.vnet"] = "/dev/vmnet2"
      end
#      mgmt_config.vm.provision :shell, path: "bootstrap-mgmt.sh"
  end

  # create some rhel servers

  (1..2).each do |i|
    config.vm.define "ansible#{i}" do |rhel_conf|
        rhel_conf.vm.box = "generic/centos8"
        rhel_conf.vm.hostname = "ansible#{i}"
        rhel_conf.vm.network :private_network, ip: "192.168.180.3#{i}"
        rhel_conf.vm.provider "vmware_workstation" do |vb|
          vb.vmx["memsize"] = 512
          vb.vmx["ethernet1.vnet"] = "/dev/vmnet2"
        end
    end
  end


  config.vm.define :ansible3 do |n1|
	n1.vm.synced_folder ".", "/vagrant", disabled: false
	n1.vm.box = "generic/centos8"
	n1.vm.hostname = "ansible3"
    n1.vm.network :private_network, ip: "192.168.180.33"
	n1.vm.provider "vmware_workstation" do |v1|

	  dir = "/home/kb/vagrant/rh407/2G-disk"

	  unless File.directory?( dir )
		Dir.mkdir dir
          end

	  file_to_disk = "#{dir}/2G-disk.vmdk"

	  unless File.exists?( file_to_disk )
		`/usr/bin/vmware-vdiskmanager -c -s 2GB -a lsilogic -t 1 #{file_to_disk}`
	  end

	  swp1 = "/home/kb/vagrant/rh407/1G-swap1"
	  swp2 = "/home/kb/vagrant/rh407/1G-swap2"

          unless File.directory?( swp1 )
		Dir.mkdir swp1
	  end

	  file_to_swap1 = "#{swp1}/1G-swap1.vmdk"

	  unless File.directory?( swp2 )
		Dir.mkdir swp2
	  end

	  file_to_swap2 = "#{swp2}/1G-swap2.vmdk"

	  unless File.exists?( file_to_swap1 )
		`/usr/bin/vmware-vdiskmanager -c -s 1GB -a lsilogic -t 1 #{file_to_swap1}`
	  end

	  unless File.exists?( file_to_swap2 )
		`/usr/bin/vmware-vdiskmanager -c -s 1GB -a lsilogic -t 1 #{file_to_swap2}`
	  end

	  v1.vmx["scsi0:1.filename"] = file_to_disk
	  v1.vmx["scsi0:1.present"] = 'TRUE'
	  v1.vmx["scsi0:1.redo"] = ''
	  v1.vmx["scsi0:2.filename"] = file_to_swap1
	  v1.vmx["scsi0:2.present"] = 'TRUE'
	  v1.vmx["scsi0:2.redo"] = ''
	  v1.vmx["scsi0:3.filename"] = file_to_swap2
	  v1.vmx["scsi0:3.present"] = 'TRUE'
	  v1.vmx["scsi0:3.redo"] = ''
	  v1.vmx["memsize"] = 1024
	  v1.vmx["numvcpus"] = 1
      v1.vmx["ethernet.vnet"] = "/dev/vmnet2"
#     v1.vmx["sata0:1.present"] = 'TRUE'
#     v1.vmx["sata0:1.deviceType"] = 'cdrom-image'
#     v1.vmx["sata0:1.fileName"] = '/usr/lib/vmware/isoimages/linux.iso'
#     v1.vmx["ethernet1.addresstype"] = "generated"
#     v1.vmx["ethernet1.connectiontype"] = "nat"
#     v1.vmx["ethernet1.present"] = "TRUE"
#     v1.vmx["ethernet1.virtualdev"] = "e1000"
#     v1.vmx["ethernet2.addresstype"] = "generated"
#     v1.vmx["ethernet2.connectiontype"] = "nat"
#     v1.vmx["ethernet2.present"] = "TRUE"
#     v1.vmx["ethernet2.virtualdev"] = "e1000"
	end
  end
end
