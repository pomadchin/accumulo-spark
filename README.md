# Apache Accumulo Spark Multinode Cluster with Docker.

Docker containers with prepeared environment to run Geotrellis jobs.
As the result, there would be three containers (two slaves and one master) on a single machine in a distributed mode,
so for heavy geotrellis tasks there should be enough ram.

## Build Multinode HDFS + Accumulo + Spark Cluster

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

## Interaction example

* Fix `./start-cluster.sh` (for example to forward volume inside containers)
  * ```bash
       #!/bin/bash
       docker run -d -t --dns 127.0.0.1 -v /localFolder:/dockerFolder -e NODE_TYPE=s -e ZOOKEEPER_ID=2 --name slave1 -h slave1.owm daunnc/hadoop-dn:0.2 FIRST_IP=$(docker inspect --format="{{.NetworkSettings.IPAddress}}" slave1)
       
       docker run -d -t --dns 127.0.0.1 -v /localFolder:/dockerFolder -e NODE_TYPE=s -e ZOOKEEPER_ID=3 -e JOIN_IP=$FIRST_IP --name slave2 -h slave2.owm daunnc/hadoop-dn:0.2

       docker run -d -t --dns 127.0.0.1 -v /localFolder:/dockerFolder -e NODE_TYPE=m -e ZOOKEEPER_ID=1 -e JOIN_IP=$FIRST_IP -p 4444:4444 -p 9000:9000 -p 50010:50010 -p 50020:50020 -p 50070:50070 -p 50075:50075 -p 50090:50090 -p 50475:50475 -p 8030:8030 -p 8031:8031 -p 8032:8032 -p 8033:8033 -p 8040:8040 -p 8042:8042 -p 8060:8060 -p 8088:8088 -p 50060:50060 -p 2181:2181 -p 2888:2888 -p 3888:3888 -p 9999:9999 -p 9997:9997 -p 50091:50091 -p 50095:50095 -p 4560:4560 -p 12234:12234 -p 8080:8080 -p 8081:8081 -p 7077:7077 -p 4040:4040 -p 4041:4041 -p 1808:1808 --name master1 -h master1.owm daunnc/hadoop-nn-dn:0.2
    ```

* Get inside master container: `docker exec -it master1 /bin/bash`
* Login as an _hduser_ `su - hduser` to run jobs
* Run job via `spark-submit`, using jars and scripts from the forwarded folder (`/dockerFolder`)
     
## License

* Based on a repository: https://github.com/alvinhenrick/hadoop-mutinode
* Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
