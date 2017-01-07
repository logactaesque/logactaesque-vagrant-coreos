# CoreOS Vagrant for Logactaesque

This experimental repo provides a basic Vagrantfile to create CoreOS virtual machine using the VirtualBox software hypervisor for use with the Logactaesque Dice Service. Its is based upon https://github.com/coreos/coreos-vagrant.

The enclosed Vagrantfile launches a coreos-based cluster comprising three nodes that run an embryonic version of the Logactaesque Dice Service. This is achieved by editing `user-data` and `config.rb` to specify instance count, memory use and instance name.

At present it creates three nodes via Vagrant and VirtualBox:
* logactaesque-coreos-01
* logactaesque-coreos-02
* logactaesque-coreos-03

## Dependencies

* [VirtualBox](https://www.virtualbox.org/) 4.3.10 or greater.
* [Vagrant](https://www.vagrantup.com/about.html) 1.8.5 or greater

## Startup and SSH

To run the VM's and connect to the first node in the cluster:
```
vagrant up
vagrant status
vagrant ssh logactaesque-coreos-01
```

## CoreOS
The configuration leverages CoreOS, meaning that from a single node, I can lists all nodes in the cluster:
```
fleetctl list-machines
```
The Logactaesque Dice Service is a service that runs on all three nodes via [systemd units](https://coreos.com/os/docs/latest/using-systemd-drop-in-units.html). A `dice.service` file exists which should be placed on a node in the folder /etc/systemd/system (as a sudo user), then run:
```
systemctl enable dice.service -- run the service
systemctl status dice.service -- get the service status
journalctl -f -u dice.service -- see a log of the service
```
