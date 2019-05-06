Vagrant.configure("2") do |config|
  config.ssh.username = "vagrant"
  config.ssh.insert_key = false

  config.vm.define :osinstaller do |uosi|
    uosi.vm.hostname = :osinstaller
    uosi.vm.box = "generic/ubuntu1804"
    uosi.vm.provider :virtualbox do |virthost|
      virthost.memory = 2048
      virthost.cpus = 2
    end

    uosi.vm.provider :libvirt do |virthost|
      virthost.memory = 2048
      virthost.cpus = 2
    end

    uosi.vm.network :private_network, ip: "192.168.80.20", netmask: "255.255.255.128"
    uosi.vm.network :private_network, ip: "192.168.85.20", netmask: "255.255.255.128"
  end

  config.vm.define :oscontroller do |uosc|
    uosc.vm.hostname = :oscontroller
    uosc.vm.box = "generic/ubuntu1804"
    uosc.vm.provider :virtualbox do |virthost|
      virthost.customize ["modifyvm", :id, "--natsettings1", "1400,0,0,0,0"]
      virthost.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      virthost.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
      virthost.memory = 8192
      virthost.cpus = 2

      unless File.exist?("/datas/os/disk_lxc.vdi")
        virthost.customize [ "createhd", "--filename", "/datas/os/disk_lxc.vdi", "--size", "153600" ]
      end
      virthost.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 3, "--device", 0, "--type", "hdd", "--medium", "/datas/os/disk_lxc.vdi" ]
     end

    uosc.vm.provider :libvirt do |virthost|
      virthost.memory = 8192
      virthost.cpus = 2
      virthost.volume_cache = "writeback"
      virthost.storage :file, size: '150G', type: 'qcow2', path: 'disk_lxc.img'
    end

    uosc.vm.network :private_network, ip: "192.168.80.70", netmask: "255.255.255.128"
    uosc.vm.network :private_network, ip: "192.168.85.70", netmask: "255.255.255.128"
  end

  config.vm.define :oscompute1 do |uosnv|
    uosnv.vm.hostname = :oscompute1
    uosnv.vm.box = "generic/ubuntu1804"
    uosnv.vm.provider :virtualbox do |virthost|
      virthost.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      virthost.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
      virthost.auto_nat_dns_proxy = false
      virthost.memory = 36864
      virthost.cpus = 6

      unless File.exist?("/datas/os/disk_nova1.vdi")
        virthost.customize [ "createhd", "--filename", "/datas/os/disk_nova1.vdi", "--size", "409600" ]
      end
      virthost.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 3, "--device", 0, "--type", "hdd", "--medium", "/datas/os/disk_nova1.vdi" ]
    end

    uosnv.vm.provider :libvirt do |virthost|
      virthost.memory = 36864
      virthost.cpus = 6
      virthost.nested = true
      virthost.driver = 'kvm'
      virthost.cpu_mode = "host-model"
      virthost.cpu_feature :name => 'vmx', :policy => 'require'
      virthost.volume_cache = "writeback"
      virthost.storage :file, size: '400G', type: 'qcow2', path: 'disk_nova1.img'
    end

    uosnv.vm.network :private_network, :ip => "192.168.80.75", :netmask => "255.255.255.128"
    uosnv.vm.network :private_network, :ip => "192.168.85.75", :netmask => "255.255.255.128"
  end

  config.vm.define :oscompute2 do |uosnv|
    uosnv.vm.hostname = :oscompute2
    uosnv.vm.box = "generic/ubuntu1804"
    uosnv.vm.provider :virtualbox do |virthost|
      virthost.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      virthost.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
      virthost.auto_nat_dns_proxy = false
      virthost.memory = 36864
      virthost.cpus = 6

      unless File.exist?("/datas/os/disk_nova2.vdi")
        virthost.customize [ "createhd", "--filename", "/datas/os/disk_nova2.vdi", "--size", "409600" ]
      end
      virthost.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 3, "--device", 0, "--type", "hdd", "--medium", "/datas/os/disk_nova2.vdi" ]
   end

   uosnv.vm.provider :libvirt do |virthost|
     virthost.memory = 36864
     virthost.cpus = 6
     virthost.nested = true
     virthost.driver = 'kvm'
     virthost.cpu_mode = "host-model"
     virthost.cpu_feature :name => 'vmx', :policy => 'require'
     virthost.volume_cache = "writeback"
     virthost.storage :file, size: '400G', type: 'qcow2', path: 'disk_nova2.img'
   end

   uosnv.vm.network :private_network, :ip => "192.168.80.76", :netmask => "255.255.255.128"
   uosnv.vm.network :private_network, :ip => "192.168.85.76", :netmask => "255.255.255.128"
 end

  config.vm.define :osstorage do |uosst|
     uosst.vm.hostname = :osstorage
     uosst.vm.box = "generic/ubuntu1804"
     uosst.vm.provider :virtualbox do |virthost|
       virthost.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
       virthost.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
       virthost.auto_nat_dns_proxy = false
       virthost.memory = 4096
       virthost.cpus = 2

       unless File.exist?("/datas/os/disk_cinder.vdi")
         virthost.customize [ "createhd", "--filename", "/datas/os/disk_cinder.vdi", "--size", "409600" ]
       end
       virthost.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 3, "--device", 0, "--type", "hdd", "--medium", "/datas/os/disk_cinder.vdi" ]
    end

    uosst.vm.provider :libvirt do |virthost|
      virthost.memory = 4096
      virthost.cpus = 2
      virthost.storage :file, size: '400G', type: 'qcow2', path: 'disk_cinder.img'
    end

    uosst.vm.network :private_network, :ip => "192.168.80.80", :netmask => "255.255.255.128"
    uosst.vm.network :private_network, :ip => "192.168.85.80", :netmask => "255.255.255.128"
  end
end
