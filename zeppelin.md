# SPARK - ZEPPELIN

### Port
Zeppelin notebook is available by default on port `8080`.  You can change this to `80` (HTTP) or `443` (TLS) if you wish, or change to another number if you are running it on the same server as **Spark Master**, which uses `8080` for the Web UI.

### 1) Setup Zeppelin
```sh
# Install Java
sudo apt install default-jre

# Download Zeppelin (check the latest version and mirror from Zepplin website)
wget url-to-mirror/zeppelin-0.7.0-bin-all.tgz

# Extract Zeppelin
tar zxvf zeppelin-0.7.0-bin-all.tgz

# Start Zeppelin
bin/zeppelin-daemon.sh start

# Stop Zepplin
bin/zeppelin-daemon.sh stop

```

### 2) Connect to Spark Standalone Cluster

Stop Zeppelin
```sh
# stop zeppelin
./bin/zeppelin-daemon.sh stop
```

Make a copy of the Zeppelin env file
```sh
cp conf/zeppelin-env.sh.template conf/zeppelin-env.sh
```

In the Zeppelin env file, export the path of the Spark Home directory. You may also change the default port of Zeppelin notebook so it does not conflict with Spark Master Web UI.
```js
export ZEPPELIN_PORT=7070
export SPARK_HOME=/path/to/spark-2.1.1-bin-hadoop2.7
```

Start Zepplin again
```sh
# start zepplin
./bin/zeppelin-daemon.sh start
```
