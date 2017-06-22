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
sbin\start-slaves.sh  <MASTER_URL>
```
The default port for master webUI is `8080` and listening on is `7070`.
You can get `<MASTER_URL>` from the logs.  It will look like spark://hostname:7070

The slave WebUS is port `8081`

4) Create a SSH RSA key pair to use for accessing slaves.  Do not use a password for the private key.  
```
ssh-keygen -t rsa -b 4096 -C "sparkuser" -f ~/.ssh/sparkuser
```

5) On each slave machine, create a new user. Use the same username for all slaves.  eg. sparkuser
```
adduser --disabled-password sparkuser

```

6) Copy public key (from the master) to the `authorized_keys` file on each slave

7) Back on the master, copy spark distribution and conf file to each slave



