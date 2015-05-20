# Apache Accumulo Spark Multinode Cluster with Docker.

Docker containers with prepeared environment to run Geotrellis jobs.
As the result, there would be three containers (two slaves and one master) on a single machine in a distributed mode,
so for heavy geotrellis tasks there should be enough ram.

## Build Multinode HDFS + Accumulo + Spark Cluster

* Build serf container
  * `cd accumulo-spark/serf`
  * `docker build -t daunnc/serf:latest .`

* Build hadoop-base container
  * `cd accumulo-spark/as-base`
  * `docker build -t daunnc/as-base:latest .`  
  
* Build hadoop-dn Slave container (DataNode / NodeManager)
  * `cd accumulo-spark/as-slave`
  * `docker build -t daunnc/as-slave-512m3:latest .`  

* Build hadoop-nn-dn Master container (NameNode / DataNode / Resource Manager / NodeManager)
  * `cd accumulo-spark/as-master`
  * `docker build -t daunnc/as-master-512m3:latest .` 

**Sart the containers.**

 * Run `./start-cluster.sh`

## Interaction example

* Fix `./start-cluster.sh` (for example to forward volume inside containers): `docker run -d -t --dns 127.0.0.1 -v /localFolder:/dockerFolder ...`
* Get inside master container: `docker exec -it master1 /bin/bash`
* Login as an _hduser_ `su - hduser` to run jobs
* Run job via `spark-submit`, using jars and scripts from the forwarded volume (`/dockerFolder`)
     
## License

* Based on a repository: https://github.com/alvinhenrick/hadoop-mutinode
* Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
