You need to use LXC device proxies in order to open certain ports on your LXD bridged network.. and route those ports into the JUJU + LXD based instances

We wrote a little template script you can use `setup_ports.sh`, it does the following for you 

```
lxc config device add <LXC NAME> <DEVICE_NAME> listen=<YOUR_LXD_HOST>:PORT connect=<JUJU_PORT_TO FORWARD_TO>:<PORT>
```

So for PostgreSQL to be reachable on your lan, we need to forward your LXDs port into the JUJU image.. notice from the juju status above that the image has port `5432/tcp` open.  Here's how we'd route our host.. to forward into that juju machine

```
lxc config device add juju-60307e-154 mysqlproxy5432 proxy listen=tcp:0.0.0.0:5432 connect=tcp:127.0.0.1:5432
```

For thrift, it's a little bit different.. due to the way thrift is coded to listen on it's ACTUAL host ip.. instead of just `0.0.0.0` so we need to put the proper ip address when we get our proxy setup.

```
lxc config device add juju-60307e-155 thriftproxy9090 proxy listen=tcp:0.0.0.0:9090 connect=tcp:10.196.87.137:9090
```
