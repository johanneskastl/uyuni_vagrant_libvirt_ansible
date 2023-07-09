# uyuni_vagrant_libvirt_ansible

This Vagrant setup creates a VM and configures [a Uyuni
server](https://www.uyuni-project.org). It also creates a VM that can be
used as a client and can be registered to Uyuni (manually, there is no full
automation yet).

Default OS is openSUSE Leap 15.4. Although that can be changed in the
Vagrantfile, please beware that this will break the Ansible provisioning.

## Vagrant

1. You need vagrant obviously. And ansible. And git...
1. Fetch the box, per default this is `opensuse/Leap-15.5.x86_64`, using
   `vagrant box add opensuse/Leap-15.5.x86_64`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. Find out the uyuniserver01 VM's IP (e.g. `vagrant address uyuniserver01`, if
   you have vagrant address installed) and connect to `https://IP:80`. You will
   need to make an exception for the self-signed TLS certificate used by
   default.
1. Log in as `admin` user (password: `admin`).
1. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to install teleport (because you want to install
it yourself), just comment out the following lines in the `Vagrantfile`:

```hcl
        node.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.limit = "all"
          ansible.groups = {
            "uyuni_clients"  => [ "uyuni-client01" ],
            "uyuni_servers"  => [ "uyuniserver01" ],
          }
          ansible.playbook = "ansible/playbook-all_nodes.yml"

        end # node.vm.provision
```

You also find all of the playbooks in the `ansible` folder.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via kastl@b1-systems.de.
