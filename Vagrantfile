Vagrant.configure("2") do |config|

  ###################################################################################
  # define number of clients
  W = 1

  # define number of uyunimasters
  M = 1

  ###################################################################################
  # provision W VMs as clients
  (1..W).each do |i|

    # disable the /vagrant folder, as / is read-only
    config.vm.synced_folder ".", "/vagrant", disabled: true

    # name the VMs
    config.vm.define "uyuni-client0#{i}" do |node|

      # which image to use
      node.vm.box = "opensuse/Leap-15.4.x86_64"

      # sizing of the VMs
      node.vm.provider "libvirt" do |lv|
        lv.random_hostname = true
        lv.memory = 1024
        lv.cpus = 1
      end

      # set the hostname
      node.vm.hostname = "uyuni-client#{i}"

    end # config.vm.define clients

  end # each-loop clients

  ###################################################################################
  # provision N VMs as uyunimasters
  (1..M).each do |i|

    # disable the /vagrant folder, as / is read-only
    config.vm.synced_folder ".", "/vagrant", disabled: true

    # name the VMs
    config.vm.define "uyunimaster0#{i}" do |node|

      # which image to use
      node.vm.box = "opensuse/Leap-15.4.x86_64"

      # sizing of the VMs
      node.vm.provider "libvirt" do |lv|
        lv.random_hostname = true
        lv.memory = 4096
        lv.cpus = 2
      end

      # set the hostname
      node.vm.hostname = "uyunimaster0#{i}"

      # if this is the last machine
      if i == M

        #################################################
        # only target one node to not run ansible twice
        # but do not limit this to this node (set ansible.limit to all)
        node.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.limit = "all"
          ansible.playbook = "ansible/playbook-all_nodes.yml"

        end # node.vm.provision

      end # if-condition

    end # config.vm.define masters

  end # each-loop masters

end # Vagrant.configure
