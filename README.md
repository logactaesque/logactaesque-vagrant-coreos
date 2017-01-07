# CoreOS Vagrant for Logactaesque

This experimental repo provides a basic Vagrantfile to create CoreOS virtual machine using the VirtualBox software hypervisor for use with the Logactaesque Dice Service.

## Dependencies

* [VirtualBox][https://www.virtualbox.org/] 4.3.10 or greater.
* [Vagrant][https://www.vagrantup.com/about.html] 1.8.5 or greater.

## Startup and SSH

```
vagrant up
vagrant ssh
```

## Provisioning with user-data

The Vagrantfile provision CoreOS VM(s) with [coreos-cloudinit][https://github.com/coreos/coreos-cloudinit] when a `user-data` file is found in the project directory. coreos-cloudinit simplifies the provisioning process through the use of a script or cloud-config document.


#### Configuration

The Vagrantfile parses `config.rb` file containing a set of options used to configure your CoreOS cluster.

## Cluster Setup

Launching a CoreOS cluster on Vagrant is as simple as configuring `$num_instances` in a `config.rb` file to 3 (or more!) and running `vagrant up`.
Make sure you provide a fresh discovery URL in your `user-data` if you wish to bootstrap etcd in your cluster.
