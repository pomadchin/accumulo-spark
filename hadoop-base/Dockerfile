FROM daunnc/serf

MAINTAINER Pomadchin Grigory, daunnc@owm.io

# Update Ubuntu
RUN apt-get update && apt-get upgrade -y

RUN apt-get install -y maven llvm-gcc build-essential zlib1g-dev make cmake pkg-config libssl-dev automake autoconf curl zip unzip

# Add oracle java 7 repository
RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get -y update

# Accept the Oracle Java license
RUN echo "oracle-java7-installer shared/accepted-oracle-license-v1-1 boolean true" | debconf-set-selections

# Install Oracle Java
RUN apt-get -y install oracle-java7-installer

RUN update-alternatives --display java

ENV JAVA_HOME /usr/lib/jvm/java-7-oracle/ 
ENV PATH $PATH:$JAVA_HOME/bin

RUN addgroup hadoop
RUN useradd -d /home/hduser -m -s /bin/bash -G hadoop hduser

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN su hduser -c "ssh-keygen -t rsa -f ~/.ssh/id_rsa -P ''"
RUN su hduser -c "cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys" 
ADD config/ssh_config ./ssh_config
RUN mv ./ssh_config /home/hduser/.ssh/config

# Hadoop 2.6.0
RUN curl -s http://www.eu.apache.org/dist/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz | tar -xz -C /usr/local
RUN ln -s /usr/local/hadoop-2.6.0 /usr/local/hadoop

# fixing the libhadoop.so like a boss
RUN rm  /usr/local/hadoop/lib/native/*
RUN curl -Ls http://dl.bintray.com/sequenceiq/sequenceiq-bin/hadoop-native-64-2.6.0.tar | tar -x -C /usr/local/hadoop/lib/native/

RUN chown -R hduser:hadoop /usr/local/hadoop-2.6.0

ADD config/bashrc /home/hduser/.bashrc
ADD config/bashrc /root/.bashrc
RUN mkdir -p /home/hduser/hdfs/datanode
RUN mkdir -p /home/hduser/hdfs/namenode
RUN chown -R hduser:hadoop /home/hduser/hdfs

RUN rm -f /usr/local/hadoop/etc/hadoop/hadoop-env.sh 
ADD config/hadoop-env.sh /usr/local/hadoop/etc/hadoop/hadoop-env.sh 

# Zookeeper 3.4.6
RUN curl -s http://mirror.cc.columbia.edu/pub/software/apache/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz | tar -xz -C /usr/local
RUN ln -s /usr/local/zookeeper-3.4.6 /usr/local/zookeeper
ADD zookeeper/zoo.cfg /usr/local/zookeeper/conf/zoo.cfg
ADD zookeeper/zookeeper-env.sh /usr/local/zookeeper/conf/zookeeper-env.sh
RUN chown -R hduser:hadoop /usr/local/zookeeper
RUN mkdir -p /var/zookeeper
RUN mkdir -p /var/log/zookeeper
RUN chown -R hduser:hadoop /var/zookeeper
RUN chown -R hduser:hadoop /var/log/zookeeper

# Accumulo 1.6.2
RUN curl -s http://archive.apache.org/dist/accumulo/1.6.2/accumulo-1.6.2-bin.tar.gz | tar -xz -C /usr/local
RUN ln -s /usr/local/accumulo-1.6.2 /usr/local/accumulo
RUN rm -r /usr/local/accumulo/logs && mkdir /usr/local/accumulo/logs 
RUN mkdir -p /usr/local/accumulo/walogs
RUN chown -R hduser:hadoop /usr/local/accumulo/

# Spark 1.3.1
RUN curl -s http://d3kbcqa49mib13.cloudfront.net/spark-1.3.1-bin-hadoop2.6.tgz | tar -xz -C /usr/local/
RUN ln -s /usr/local/spark-1.3.1-bin-hadoop2.6 /usr/local/spark
RUN chown -R hduser:hadoop /usr/local/spark
RUN mkdir -p /home/hduser/spark/tmp
RUN chown -R hduser:hadoop /home/hduser/spark/tmp
RUN mkdir -p /home/hduser/spark/work
RUN chown -R hduser:hadoop /home/hduser/spark/work

RUN echo -e "\n* soft nofile 65536\n* hard nofile 65536" >> /etc/security/limits.conf

EXPOSE 22
