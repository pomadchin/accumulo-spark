# Accumulo Spark Multinode Cluster with Docker.

Build Multinode HDFS + Accumulo + Spark Cluster
------------------------------

* Build serf container
  * Change directory to hadoop-mutinode/serf.
  * Run `docker build -t daunnc/serf .`

* Build hadoop-base container
  * Change directory to hadoop-mutinode/hadoop-base.
  * Run `docker build -t daunnc/hadoop-base:0.2 .`
  * This will take a while to build the container go grab a cup of coffee or whatever drink you like :)
  
* Build hadoop-dn Slave container (DataNode / NodeManager)
  * Change directory to hadoop-mutinode/hadoop-dn.
  * Run `docker build -t daunnc/hadoop-dn:0.2 .`  

* Build hadoop-nn-dn Master container (NameNode / DataNode / Resource Manager / NodeManager)
  * Change directory to hadoop-mutinode/hadoop-nn-dn.
  * Run `docker build -t daunnc/hadoop-nn-dn:0.2 .`  

**Sart the containers.**

 * Change directory to hadoop-mutinode.
 * Run `./start-cluster.sh`
     
## License

* Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0

* Based on a repository: https://github.com/alvinhenrick/hadoop-mutinode
