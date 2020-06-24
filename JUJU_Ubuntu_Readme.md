# Juju common commands and what they do


### working with bundles

to create a new bundle off an existing juju model

`juju export-bundle --filename config.yaml.template`

deploy a bundle in your cwd

`juju deploy ./config.yaml`


## debug-log levels

`juju model-config logging-config="<root>=WARNING;unit=TRACE"`

to watch just one module instead of the entire log

`juju model-config --include-module sample-thrift-charm`

## postgresql specific commands

the postgresql charm out of the box is not really useable locally.. it disallows connections

you must do this from juju 

`juju deploy postgresql pg-a`

then

`juju config postgresql extra_pg_auth="host all all 0.0.0.0/0 md5"`

you can then either

```
juju ssh <machine_num_of_postgresql>

```
and from within the postgresql machine
```
sudo su postgres
psql
> \password
```

to change the default password 


## some typical devices you add

`lxc config device add <LXC_IMAGE_NAME> myport9090 proxy listen=tcp:0.0.0.0:9090 connect=tcp:127.0.0.1:9090`
