# Containers


## Build the `full.recipe` image

```
$ mkdir my_working_directory
$ cd my_working_directory

# echo create ssh keys pair
$ ssh-keygen -f client_ssh.slurm_id_rsa

$ touch init_db_server
# edit your init_db_server

$ sudo singularity build full.simg full.recipe
```

An `init_db_server` example:

```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('my-secret-pw');
CREATE USER 'simple'@'localhost' IDENTIFIED BY 'my-simple-pw';
CREATE DATABASE IF NOT EXISTS local_db;
GRANT ALL PRIVILEGES ON local_db.* TO 'simple'@'%';
DROP DATABASE IF EXISTS test;

```
