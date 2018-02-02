# mesos-demo
Artifacts used to do a basic mesos POC / demo

## Dependencies

* Docker Engine 1.13.0+
* Docker Compose 1.10.0+

## QuickStart

* Clone the repo: `git clone git@github.com:brosenfi/mesos-demo.git && cd mesos-demo`
* Build the local environment: `docker-compose build`
* Run the local environment: `docker-compose up -d`
* Web Browser Access:
  - Mesos WebUI: [http://127.0.0.1:5050](http://127.0.0.1:5050)
  - Marathon WebUI: [http://127.0.0.1:8090/ui/#/apps](http://127.0.0.1:8090/ui/#/apps)
  - Chronos WebUI: [http://127.0.0.1:8888](http://127.0.0.1:8888)
  - Jupyter Lab WebUI: [http://127.0.0.1:8889](http://127.0.0.1:8889)
* Load up a service in Marathon
  - Load via Marathon api using curl: `curl -X POST http://localhost:8090/v2/apps -d @basic_nginx_server.json -H "Content-type: application/json"`
  - Navigate to the Marathon web UI, look at the application, scale it up / down
  - Use the URL's located on the Marathon web UI to navigate to the running nginx instances
  - Navigate to the Mesos web UI, notice the long-running tasks
* Run some tasks using chronos
  - Navigate to the Chronos web UI
  - Define a task manually with some given schedule, command can be something as simple as `ps -ef`
  - Define another task manually, making the last task the parent - some other simple command like `ls -alt`
  - Force the first task to run - notice both in the chain execute
  - View the graph
* Run some PySpark commands via a Jupyter notebook
  - Navigate to the Jupyter Lab web UI and select a Python notebook
  - Enter the script as follows:
```
import os
# make sure pyspark tells workers to use python3 not 2 if both are installed
os.environ['PYSPARK_PYTHON'] = '/usr/bin/python3'

import pyspark
conf = pyspark.SparkConf()

# point to mesos master or zookeeper entry (e.g., zk://10.10.10.10:2181/mesos)
conf.setMaster("mesos://zk://zookeeper:2181/mesos")
# point to spark binary package in HDFS or on local filesystem on all slave
# nodes (e.g., file:///opt/spark/spark-2.2.0-bin-hadoop2.7.tgz)
conf.set("spark.executor.uri", "http://web/spark-2.2.1-bin-hadoop2.7.tgz")
# set other options as desired
conf.set("spark.executor.memory", "2g")
conf.set("spark.core.connection.ack.wait.timeout", "1200")

# create the context
sc = pyspark.SparkContext(conf=conf)

# do something to prove it works
rdd = sc.parallelize(range(100000000))
rdd.sumApprox(3)
```
  - Execute more tests as desired and take a look at the mesos web UI for task logging
  - Finally get rid of the spark context:
```
# get rid of the spark context
sc.stop()
```

