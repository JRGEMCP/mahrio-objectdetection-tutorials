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

## Launch a LXC VM

Now we have LXD, and LXC installed and ready to use.. go ahead and instantiate a VM by hand

```
lxc launch ubuntu:18.04 your-app
```

to enter the VM you can perform the following

```
lxc exec your-app /bin/bash
root@your-app:~# 
```

you now have root, inside `your-app` LXC VM

you can install flask, setup python etc..

```
root@your-app:~# apt update
root@your-app:~# apt upgrade 
root@your-app:~# apt install vim python python-pip
```

Create a simple flask server likeso

```
from flask import Flask 
app = Flask(__name__)

@app.route("/") 
def hello():
    return "Hello World!"

if __name__ == "__main__": 
    app.run(host='0.0.0.0', port=5000)
```


```
root@your-app:~# vim test_app.py
```
