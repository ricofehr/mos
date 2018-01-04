Vagrant.configure("2") do |config|
    config.ssh.username = "vagrant"
    config.ssh.insert_key = false

    config.vm.define :oscontroller do |uosc|
      uosc.vm.hostname = :oscontroller
      uosc.vm.box = "bento/ubuntu-16.04"
      uosc.vm.provider :virtualbox do |virthost|
        #virthost.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
        #virthost.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
        #virthost.customize ["modifyvm", :id, "--nictype1", "virtio"]
        virthost.memory = 8192
        virthost.cpus = 2
      end

        uosc.vm.network :private_network, ip: "192.168.80.70", netmask: "255.255.255.128"
        uosc.vm.network :private_network, ip: "192.169.80.70", netmask: "255.255.255.128"
    end

   config.vm.define :oscompute1 do |uosnv|
      uosnv.vm.hostname = :oscompute1
      uosnv.vm.box = "bento/ubuntu-16.04"
 
      uosnv.vm.provider :virtualbox do |virthost|
        virthost.customize ["modifyvm", :id, "--natsettings1", "1400,0,0,0,0"]
        virthost.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
        #virthost.customize ["modifyvm", :id, "--nictype1", "virtio"]
        virthost.auto_nat_dns_proxy = false
        virthost.memory = 8192
        virthost.cpus = 8
      end

      uosnv.vm.network :private_network, :ip => "192.168.80.75", :netmask => "255.255.255.128"
      uosnv.vm.network :private_network, :ip => "192.169.80.75", :netmask => "255.255.255.128"
  end

   config.vm.define :oscompute2 do |uosnv|
      uosnv.vm.hostname = :oscompute2
      uosnv.vm.box = "bento/ubuntu-16.04"
      uosnv.vm.provider :virtualbox do |virthost|
        #virthost.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
        #virthost.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
        virthost.customize ["modifyvm", :id, "--natsettings1", "1400,0,0,0,0"]
        virthost.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
        #virthost.customize ["modifyvm", :id, "--nictype1", "virtio"]
        virthost.auto_nat_dns_proxy = false
        virthost.memory = 8192
        virthost.cpus = 8
     end

      uosnv.vm.network :private_network, :ip => "192.168.80.76", :netmask => "255.255.255.128"
      uosnv.vm.network :private_network, :ip => "192.169.80.76", :netmask => "255.255.255.128"
    end
end
