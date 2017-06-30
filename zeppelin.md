# SPARK - ZEPPELIN

### Port
Zeppelin notebook is available by default on port `8080`.  You can change this to `80` (HTTP) or `443` (TLS) if you wish, or change to another number if you are running it on the same server as **Spark Master**, which uses `8080` for the Web UI.

### Setup Zeppelin
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

### Connect to Spark Standalone Cluster
This is a 2 step process.

  Step 1) Add the Spark Home to Zeppelin
  Stop Zeppelin
  ```sh
  # stop zeppelin
  ./bin/zeppelin-daemon.sh stop
  ```

  Make a copy of the Zeppelin env file
  ```sh
  cp conf/zeppelin-env.sh.template conf/zeppelin-env.sh
  ```

  In the Zeppelin env file, export the path of the Spark Home directory. You may also change the default port of Zeppelin notebook so it does not conflict with Spark Master Web UI.  You may need to download and uncompress the Spark Distribution.
  ```js
  export ZEPPELIN_PORT=7070
  export SPARK_HOME=/path/to/spark-2.1.1-bin-hadoop2.7
  ```

  Start Zepplin again
  ```sh
  # start zepplin
  ./bin/zeppelin-daemon.sh start
  ```

  Step 2) Add Spark Master URL to Zeppelin notebook Web UI
  1) Open Zeppelin in web browser
  2) Click on username (default is anonymouse), and then click interpreter
  3) Scroll down and look for Spark
  4) Click Edit
  5) Update `master` with the spark master url. The format should be `spark://domain-name-or-ip-number:port-number`
  5) Click Save button
  
  
