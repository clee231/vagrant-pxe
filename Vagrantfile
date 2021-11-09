Vagrant.configure("2") do |config|
  
  config.vm.define :server do |server|

    # Use debian 10 for server
    server.vm.box = "generic/ubuntu1804"
    server.vm.hostname = "pxe-server"

    # Create a private network without vagrant DHCP
    server.vm.network "private_network",
      ip: "172.31.0.5",
      libvirt__network_name: "pxedemo",
      libvirt__dhcp_enabled: false

    # Configure VM
    server.vm.provider :libvirt do |libvirt|
      libvirt.cpu_mode = 'host-passthrough'
      libvirt.memory = '1024'
      libvirt.cpus = '1'
    end

    # Use ansible to install server
    server.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.verbose = true
    end
  end

  # Disable autostart of client when "vagrant up"
  config.vm.define :client, autostart: false do |client|
    client.vm.hostname = "pxe-client"
    # Connect to private server private network
    client.vm.network "private_network",
      libvirt__network_name: "pxedemo"

    # Configure VM
    client.vm.provider :libvirt do |libvirt|
      libvirt.cpu_mode = 'host-passthrough'
      libvirt.memory = '2048'
      libvirt.cpus = '1'
      # Create a disk
      libvirt.storage :file,
        size: '30G',
        type: 'qcow2',
        bus: 'sata',
        device: 'sda'
      # Set pxe network NIC as default boot
      boot_network = {'network' => 'pxedemo'}
      libvirt.boot boot_network
      libvirt.boot 'hd'
      # Set UEFI boot, comment for legacy
      libvirt.loader = '/usr/share/ovmf/x64/OVMF.fd'
    end
  end
end
