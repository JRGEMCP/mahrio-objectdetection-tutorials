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
root@your-app:~# pip install flask
```

Create a simple flask server via `root@your-app:~# vim test_app.py`

```
from flask import Flask 
app = Flask(__name__)

@app.route("/") 
def hello():
    return "Hello World!"

if __name__ == "__main__": 
    app.run(host='0.0.0.0', port=5000)
```

Now startup this example server

```
root@second-app:~# python test_app.py 
 * Serving Flask app "test_app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

## Port forwarding individual containers

There are more advanced networking techniques available, so that your containers for instance can get an IP address from your home lans DHCP server.. but by default we have setup a small NAT.  All containers can see themselves and the host, use the internet, but that is it.

To open up VM containers to the outside world (Other computers in your network) 

```
lxc config device add your-app myport5000 proxy listen=tcp:0.0.0.0:5000 connect=tcp:127.0.0.1:5000
```

to remove the proxy

```
lxc config device remove my-app myport5000
```

With the device applied, your other machines will HTTP GET onto the host_ip:5000 , but the vm container will respond.. giving you a helloworld
