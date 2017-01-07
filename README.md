# CoreOS Vagrant for Logactaesque

This experimental repo provides a basic Vagrantfile which can be employed to create CoreOS virtual machines using the VirtualBox software hypervisor for use with the Logactaesque Dice Service. The work enclosed is based upon https://github.com/coreos/coreos-vagrant.

The enclosed Vagrantfile launches a CoreOS-based cluster comprising three nodes that  can run an embryonic version of the [Logactaesque Dice Service](https://github.com/logactaesque/logactaesque-dice). This is achieved by
* editing `user-data` and `config.rb` to specify instance count, memory use and instance name.
* Supplying a systemd unit file `dice.service` to the instance which can be started to run a local docker image of the Logactaesque Dice Service (available on port 8080).

At present this configuration creates three nodes via Vagrant and VirtualBox:
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
The configuration leverages CoreOS, meaning that from a single node, it is possible to see all other nodes in the cluster:

```
fleetctl list-machines
```

The Logactaesque Dice Service is a service that can be run on all three nodes via [systemd units](https://coreos.com/os/docs/latest/using-systemd-drop-in-units.html). A `dice.service` service unit file exists in the repo which pulls and runs a docker image containing the Logactaesque Dice Service, and port forwarding allow a user to reference the service directly via HTTP. The service unit file  should be placed on any node in the folder /etc/systemd/system (as a sudo user), then run (also as sudo):
```
systemctl enable dice.service -- run the service
systemctl start dice.service -- run the service
systemctl status dice.service -- get the service status
journalctl -f -u dice.service -- see a log of the service to assess if started OK
```

Where the dice service is enabled and active, it is possible to reference the service using a `curl` command
```
curl http://localhost:8080/dice/green/roll
```
