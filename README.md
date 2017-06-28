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

## Resilient Distributed Datasets (RDD)
- in-memory
- lazy evaluated 
- RDD operations can be: transformations (returns new dataset) and actions (returns values)
- lineage graph
-- see http://training.databricks.com/visualapi.pdf 

## Access Spark cluster for compute
1) Interactively query and run code on Spark cluster using `./bin/spark-shell` (Scala) and `./bin/pyspark` (Python).  Good for prototyping and debugging
2) Submit (long-running) applications to Spark cluster using `./bin/spark-submit`.  You can submit python scripts, and JAR packages (maven is recommended over sbt).  Examples https://spark.apache.org/docs/latest/submitting-applications.html
3) Notebooks like **Zepplin** can access the spark cluster by specifying the spark master URL.  **Jupyter Notebook** can access the spark cluster via pyspark (consider also using the findspark package).


## Working with data files and storage
File can be sent to the spark cluster by using the `SparkContext` class.  There are methods to make the files available on all machines or a subset of machines.  Ideally, data files should be stored on a form of distributed storage (HDFS, AWS S3, Cassandra, MongoDB, MySQL, etc.)
