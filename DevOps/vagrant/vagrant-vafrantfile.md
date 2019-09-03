# Vagrant - Vagrantfile

## Vagrant Variables
```vagrant
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.network "public_network", :bridge => 'wlp8s0', ip: "192.168.1.199"
end

```
or 

```vagrant
Vagrant.configure("2") do |config|
    config.vm.network "public_network", :bridge => 'wlp8s0', ip: "192.168.1.199"
end
```

## vagrant loop
```bash
(1..3).each do |i|
  config.vm.define "node-#{i}" do |node|
    node.vm.provision "shell",
      inline: "echo hello from node #{i}"
  end
end
```

## vagrant check for provider type
```bash
Vagrant.configure('2') do |config|
  # shared networking configuration
  network_configs = lambda do |network|
    network.vm.network :forwarded_port, guest: 1080, host: 1080
    network.vm.network :forwarded_port, guest: 3000, host: 3000
    network.vm.synced_folder '.', '/vagrant', nfs: true
  end

  config.vm.provider :virtualbox do |_, override|
    # some code
    network_configs.call override
  end

  config.vm.provider :vmware_fusion do |_, override|
    # some code
  end

  config.vm.provider :kvm do |_, override|
    # some code
    network_configs.call override
  end
end
```