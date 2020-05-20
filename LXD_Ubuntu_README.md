# Getting started with LXD on Ubuntu 20.04

LXD is a tool you can use to utilize Linux Containers (called LXC)

### Prerequisite

We assume you have already installed Ubuntu 20.04 LTS to your baremetal server

### Install via snap

```
sudo apt update 
sudo apt upgrade -y
sudo snap install lxd
sudo lxd init
```

Follow the defaults on all options EXCEPT IPV6.  Disable it, because Juju does not support it at this time

```
What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:none
```

After all that, you should now be completely ready to utilize LXC and LXD

```
$ lxc list
+---------------+---------+---------------------+------+-----------+-----------+
|     NAME      |  STATE  |        IPV4         | IPV6 |   TYPE    | SNAPSHOTS |
+---------------+---------+---------------------+------+-----------+-----------+
+---------------+---------+---------------------+------+-----------+-----------+
```

### Launch a LXC VM

