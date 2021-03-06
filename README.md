# SPARK

## Key components
- master and workers
- driver program and executors


## Explaination of key components
- Applications or interactive shells access the Spark cluster's compute and memory resources via a **Spark Context** object. The application is called the **Driver Program**.  
- The cluster manager (master) asks worker nodes (slaves) to create executor objects to do computation for job requests from the Driver Program.  


## Cluster Managers
Standalone is basic and easy to setup.  Mesos has more fine grained and dynamic scheduling of memory/cores, so is good for interactive shells like Jupyter and Zeppelin.  Yarn is integrated with HDFS, and queue based scheduling of resources.
- Standalone mode
- Mesos
- Yarn


## Ports
- Master web UI is `8080`
- Master listens on `7077` for job submission and spark slaves
- Slave web UI is `8081`
- Slave also uses a random port to talk to the Driver Program (Spark Context)
- SSH access can be setup between master and slaves to start/stop the cluster using shell script on master (sbin/start-slaves.sh)
- Master URL looks like this  `spark://hostname:7077`
- Spark context (Driver program) also has a WebUI at port `4040` so you view the event timeline.


## Setup Standalone mode

1) Setup Spark Master
```sh
#Download and uncompress pre-built spark from https://spark.apache.org/downloads.html
wget https://d3kbcqa49mib13.cloudfront.net/spark-2.1.1-bin-hadoop2.7.tgz
tar -xvzf spark-2.1.1-bin-hadoop2.7.tgz

# Install java
sudo apt install openjdk-8-jre-headless

# Create a SSH RSA key pair on the master to use for accessing slaves.  Do not use a password for the private key.  
# On Spark Master
ssh-keygen -t rsa -b 4096 -C "spark-slave" -f ~/.ssh/spark-slave

```

2) On Spark Master, create `conf/spark-env.sh` by copying `conf/spark-env.sh.template`.  Export the IP address of the Master so that it can be accessible by the slaves
```sh
export SPARK_MASTER_HOST=1.2.3.4
```

3) Now start the Spark Master
```sh
# Start Spark Master:
sbin\start-master.sh

```


3) Setup for each slave machine
```sh
# On Spark Slave

# Install java
sudo apt install openjdk-8-jre-headless

# create spark user. You may need to create the ~/.ssh directory, and authorized keys file
adduser --disabled-password sparkuser

# Copy public key from master, and add to slave's authorized key file
cat spark-slave.pub >> authorized_keys

# Download and uncompress spark distrubtion
wget https://d3kbcqa49mib13.cloudfront.net/spark-2.1.1-bin-hadoop2.7.tgz
tar -xvzf spark-2.1.1-bin-hadoop2.7.tgz

# Start Spark Slave
sbin\start-slave.sh  <MASTER_URL and PORT>

```


## Resilient Distributed Datasets (RDD)
- in-memory
- lazy evaluated 
- RDD operations can be: transformations (returns new dataset) and actions (returns values)
- lineage graph
-- see http://training.databricks.com/visualapi.pdf 

## Access Spark cluster for compute
1) Interactively query and run code on Spark cluster using `./bin/spark-shell` (Scala) and `./bin/pyspark` (Python) and `./sparkR` (R).  Good for prototyping and debugging
2) Submit (long-running) applications to Spark cluster using `./bin/spark-submit`.  You can submit python scripts, and JAR packages (maven is recommended over sbt).  Examples https://spark.apache.org/docs/latest/submitting-applications.html
3) Notebooks like **Zepplin** can access the spark cluster by specifying the spark master URL.  **Jupyter Notebook** can access the spark cluster via pyspark (consider also using the findspark package).


## Working with data files and storage
File can be sent to the spark cluster by using the `SparkContext` class.  There are methods to make the files available on all machines or a subset of machines.  Ideally, data files should be stored on a form of distributed storage (HDFS, AWS S3, Cassandra, MongoDB, MySQL, etc.)

### Access Spark Master from Jupyter (Python)
Install and configure Jupyter to find pyspark package
```sh
conda install jupyter
pip install findspark
# if spark home directory is in non-standard location
# check if spark home has already been declared
echo $SPARK_HOME
# export spark home so findspark can find it
export SPARK_HOME=/path/to/spark-home-dir
```

Python code to connect to spark
```python
import findspark
findspark.init()
import pyspark
from pyspark.conf import SparkConf
conf = SparkConf()
# your app name. This will appear on the spark master UI
conf.setAppName("test-1")
# the url of the spark master
conf.setMaster("spark://X.X.X.X:7077")
# number of workers
conf.set("spark.executor.instances", "10")
#number of cores per worker
conf.set("spark.cores.max", "2")
# amount of memory for each worker
conf.set("spark.executor.memory", "4g")
# connect to spark cluster
sc = pyspark.SparkContext(conf=conf)
# disconnect from spark cluster
sc.stop()
```

