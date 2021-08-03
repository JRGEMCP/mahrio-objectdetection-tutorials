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

Thrift device

`lxc config device add <LXC_IMAGE_NAME> myport9090 proxy listen=tcp:0.0.0.0:9090 connect=tcp:127.0.0.1:9090`

## Step 8 - Enable the PostgreSQL username password

postgresql comes with a default user `postgres` , but we need to change the password before we login.

you `juju ssh XX` into the instance running postgresql, and simply use the postgres user to change the password like so

remember this is inside the postgres
```
> sudo su postgres
> psql
postgres=# \password
```

this will prompt you to enter a password.  Change it to whatever you want and make a note of it.  You will need it later.
to quit the psql commandline just type `\q` and exit from the postgres instance.
Now setup your beginning STONKS database, `CREATE DATABASE _____` and `\l` lists your current DBs.

```
postgres=# CREATE DATABASE mydb;
CREATE DATABASE
postgres=# \l
                             List of databases
   Name    |  Owner   | Encoding | Collate | Ctype |   Access privileges   
-----------+----------+----------+---------+-------+-----------------------
 postgres  | postgres | UTF8     | C       | C     | 
 mydb      | postgres | UTF8     | C       | C     | 
 template0 | postgres | UTF8     | C       | C     | =c/postgres          +
           |          |          |         |       | postgres=CTc/postgres
 template1 | postgres | UTF8     | C       | C     | =c/postgres          +
           |          |          |         |       | postgres=CTc/postgres
(4 rows)
```

To quit the psql commandline just type `\q` and exit from the postgres instance.


