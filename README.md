# SPARK

## Cluster Managers
- Standalone mode
- Mesos
- Yarn


## Setup Standalone mode

1) Download pre-built spark from https://spark.apache.org/downloads.html

2) Untar
```
tar -xvzf spark-2.1.1-bin-hadoop2.7.tgz
```

3) You can start an individual `master` and `slave` by:
```
sbin\start-master.sh
sbin\start-slaves.sh
```
The default port for master webUI is `8080` and listening on is `7070`.

4) Create a SSH RSA key pair to use for accessing slaves.  Do not use a password for the private key.  

5) On each slave machine, create a new user. Use the same username for all slaves.  eg. sparkuser

6) Copy public key (from the master) to the `authorized_keys` file on each slave

7) Back on the master, copy spark distribution and conf file to each slave



