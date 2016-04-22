# -*- mode: ruby -*-
# vi: set ft=ruby :

# needed for linked clones to work
Vagrant.require_version ">= 1.8.0"

hosts = [ {:name => "jmaster", :ip => "10.20.9.91", :ram => 1024},
          {:name => "jslave1", :ip => "10.20.9.201", :ram => 2048},
          {:name => "jslave2", :ip => "10.20.9.202", :ram => 2048}
        ]

Vagrant.configure("2") do |config|

  # - requires the vagrant-hostmanager plugin
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  hosts.each do |host|
    config.vm.define host[:name] do |c|

      # vagrant 'feature' breaks ansible without this
      c.ssh.insert_key = false

      c.vm.box = "box-cutter/centos66"
      c.vm.box_version = "2.0.11"

      c.vm.host_name = host[:name]
      c.vm.network :private_network, ip: host[:ip], netmask: "255.255.255.0"

      c.vm.provider("virtualbox") do |vb|
        vb.memory = host[:ram]
        vb.linked_clone = true
      end

      # turn off shared folder
      c.vm.synced_folder ".", "/vagrant", :disabled => true

    end
  end
end
