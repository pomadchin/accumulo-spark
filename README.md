# Apache Accumulo Spark Multinode Cluster with Docker.

Docker containers with prepeared environment to run Geotrellis jobs.
As the result, there would be three containers (two slaves and one master) on a single machine in a distributed mode,
so for heavy geotrellis tasks there should be enough ram.

Build Multinode HDFS + Accumulo + Spark Cluster
------------------------------

* Build serf container
  * `cd accumulo-spark/serf`
  * `docker build -t daunnc/serf .`

* Build hadoop-base container
  * `cd accumulo-spark/hadoop-base`
  * `docker build -t daunnc/hadoop-base:0.2 .`  
  
* Build hadoop-dn Slave container (DataNode / NodeManager)
  * `cd accumulo-spark/hadoop-dn`
  * `docker build -t daunnc/hadoop-dn:0.2 .`  

* Build hadoop-nn-dn Master container (NameNode / DataNode / Resource Manager / NodeManager)
  * `cd accumulo-spark/hadoop-nn-dn`
  * `docker build -t daunnc/hadoop-nn-dn:0.2 .` 

**Sart the containers.**

 * Run `./start-cluster.sh`
     
## License

* Based on a repository: https://github.com/alvinhenrick/hadoop-mutinode
* Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
