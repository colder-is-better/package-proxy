# Quickly setup a Squid proxy for Linux packages

We show two simple methods for quickly setting up Squid on a Debian-based physical/virtual host, for caching packages of (ephemeral) Linux systems. To apply any of those two methods you need a fairly modern Linux system with [Ansible](https://www.ansible.com) installed. Should you chose to apply the second method, i.e., have a VM as a local proxy for other VMs, then you will also need [VirtualBox](https://www.ansible.com) and [Vagrant](http://vagrantup.com).

### Use Ansible to provision a physical host (e.g., a Raspberry Pi) or a readily available VM

In file `provision_vars.yml` modify the following parameters accordingly.

* __squid_conf_full_path:__ This is the full path of `squid.conf`, the configuration file of Squid. The default for a Debian or an Ubuntu host is `/etc/squid/squid.conf`. For a Raspberry Pi host with Raspbian, change to `/etc/squid3/squid.conf`.
* __local_network:__ The local network, in CIDR format, Squid will be operating in. Change the value of this parameter accordingly, so it makes (better) sense for your setup.
* __object_size_upper_limit:__ This places a limit (in megabytes) on the largest object that Squid can store on disk.
* __cache_size_upper_limit:__ The total amount of disk space (in megabytes) to use for storing objects.
* __cache_dir_path:__ Full path to the directory used for storing cached objects. When provisioning a Raspberry Pi, it is recommended __not__ to have this directory on the SD card but on an external disk instead.
* __use_local_squid:__ Set to `true` (default) if you want the package manager of the proxy host to also use Squid, set to `false` if you do not.

In file `hosts` set the IP or the FQDN of the proxy host (change `1.2.3.4`), also the username of the remote user account Ansible will attempt to connect to (change `userson`). Said remote user should be either `root` or one with passwordless sudo privileges. Finally, just type the following to provision the remote host:

	ansible-playbook -b -i hosts provision.yml

### Use Vagrant to automatically create a VirtualBox Debian VM with Squid preconfigured

In addition to modifying `provision_vars.yml` (see above), you will probably need to change a couple of parameters in `machine_vars.yml` also.

* __ram_size:__ How much virtual RAM, in megabytes, the VM is going to have?
* __core_count:__ How many CPU cores?
* __fqdn:__ This is the fully qualified domain name of the VM.
* __ipaddr:__ This is the (non-routable) IPv4 address of the VM. It should be reachable from the physical host and local VMs also.

To instantiate a Debian based VirtualBox VM with Squid preconfigured, just type:

	vagrant up

_**Note:** This repository has been created to complement a [HOWTO article](https://deltahacker.gr/?p=18105) for deltaHacker magazine (text in Greek, active subscription may be required)_
