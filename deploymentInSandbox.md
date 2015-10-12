---
layout: doc
title:  "Deploy Eagle in Sandbox"
permalink: /docs/deploymentInSandbox.html
---

### Pre-requisites

##### Environment
> To install eagle on a sandbox you need to run a HDP sandbox image in a virtual machine with 8GB memory recommended.
>
> 1. Get a [Virtualization environment](http://hortonworks.com/products/hortonworks-sandbox/#install) vmware or virtualbox 
> 2. Get [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox/#install) v 2.2.4 or later.

##### Start Storm, Spark, HBase & Kafka via Ambari
> 1. Add root as a HBase superuser via [Ambari](http://127.0.0.1:8080/#/main/services/HBASE/configs) (Optional, a user can operate HBase by sudo su hbase, as an alternative).
>
    ![Hbase User](/images/docs/AmbariHbaseConfig.png "Hbase User")
> 2. Start HBase, Spark, Storm & Kafka from Ambari UI. Taking Storm as an example
![Restart Services](/images/docs/startStorm.png "Services")

##### Add namenode log4j Kafka appender (For HDFS), and other option is [here](/docs/importHDFSAuditLog.html)
>
> 1. Configure Advanced hadoop-env via [Ambari UI](http://localhost:8080/#/main/services/HDFS/configs), and add a log4j appender called "KAFKA_HDFS_AUDIT".
>
        log4j.appender.KAFKA_HDFS_AUDIT=eagle.log4j.kafka.KafkaLog4jAppender
        log4j.appender.KAFKA_HDFS_AUDIT.Topic=sandbox_hdfs_audit_log
        log4j.appender.KAFKA_HDFS_AUDIT.BrokerList=sandbox.hortonworks.com:6667
        log4j.appender.KAFKA_HDFS_AUDIT.KeyClass=eagle.log4j.kafka.hadoop.AuditLogKeyer
        log4j.appender.KAFKA_HDFS_AUDIT.Layout=org.apache.log4j.PatternLayout
        log4j.appender.KAFKA_HDFS_AUDIT.Layout.ConversionPattern=%d{ISO8601} %p %c{2}: %m%n
        log4j.appender.KAFKA_HDFS_AUDIT.ProducerType=sync
        #log4j.appender.KAFKA_HDFS_AUDIT.BatchSize=1
        #log4j.appender.KAFKA_HDFS_AUDIT.QueueSize=1
> 2. Edit Advanced hadoop-env via [Ambari UI](http://localhost:8080/#/main/services/HDFS/configs), and add the reference to KAFKA_HDFS_AUDIT to HADOOP_NAMENODE_OPTS.
>
        -Dhdfs.audit.logger=INFO,DRFAAUDIT,KAFKA_HDFS_AUDIT
> 3. Edit Advanced hadoop-env via [Ambari UI](http://localhost:8080/#/main/services/HDFS/configs), and append the following command
>
        export HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/path/to/eagle/lib/log4jkafka/lib/*
> 4. restart the namenode
> 5. Validate if it works
>
* Check name node is correctly started without log4j related exception.
* Check whether logs are flowing into Topic `sandbox_hdfs_audit_log` with Kafka command line `bin/kafka-console-consumer.sh`


### Steps of Eagle Installation

Step 1: Download Eagle tarball

* Option 1: build form source code [eagle github](https://github.xyz.com/eagle/eagle/tree/release1.0), and eagle-xxx-bin.tar.gz will be generated under ./eagle-assembly/target

        mvn clean install -DskipTests=true

* Option 2: Download eagle jar from [here](http://xyz.com).

Step 2: Copy the tarball into sandbox and extract it

        # extract
        tar -zxvf eagle-0.1.0-bin.tar.gz
        mv eagle-0.1.0 /usr/hdp/current/eagle

Step 3: Install Eagle service and three monitoring topologies, including HdfsAuditLog, HiveQueryLog, and [OnlineUserProfiles](/docs/onlineUserProfiles.html)

* Option 1: start with eagle command line

      example/eagle-sandbox-starter.sh

* Option 2: start with [Eagle Ambari plugin](/docs/ambariPluginInstall.html)

Step 4: Check [Eagle service UI](http://sandbox.hortonworks.com:9099/eagle-service) and [topology UI](http://sandbox.hortonworks.com:8744) with login account `admin/secret`.

### **Q & A**

Q1. Not able to access storm worker log via browser

A1: add the following line in host machine's hosts file

      127.0.0.1 sandbox.hortonworks.com

Q2. Not able send data into kafka using kafka console producer: /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list localhost:6667 --topic sandbox_hdfs_audit_log

A2: kafka broker are binding to host sandbox.hortonworks.com

       /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list sandbox.hortonworks.com:6667 --topic sandbox_hdfs_audit_log
       cat /etc/hosts
       127.0.0.1 localhost.localdomain localhost
       10.0.2.15 sandbox.hortonworks.com sandbox ambari.hortonworks.com

Q3. Cannot open eagle service url localhost:9099

A3: If your network is NAT, then you need to add forwarding port 9099 for eagle service

