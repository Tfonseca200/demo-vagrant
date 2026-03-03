Vagrant.configure("2") do  |config| 
  config.vm.box = "generic/fedora39"

  config.vm.provider :libvirt do |lv|
    lv.uri = "qemu:///system"
    lv.cpus = 2
    lv.memory = 1000
    lv.storage :file, size: '10G', device: 'vdb'
  end


  config.vm.define "vm1" do |vm1|
    vm1.vm.hostname = "vm1"

    vm1.vm.network "private_network",
    ip: "10.10.0.40",
    libvirt__network_name: "demo_network"
  end

  config.vm.define "vm2" do |vm2|
    vm2.vm.hostname = "vm2"

    vm2.vm.network "private_network",
      ip: "10.10.0.50",
      libvirt__network_name: "demo_network"
  end
end
