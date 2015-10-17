---
layout: doc
title:  "Deploy Eagle in Sandbox"
permalink: /docs/deployment-in-sandbox.html
---


### Pre-requisites

* **Environment**

    To install eagle on a sandbox you need to run a HDP sandbox image in a virtual machine with 8GB memory recommended.

    1. Get Virtual Box or VMware [Virtualization environment](http://hortonworks.com/products/hortonworks-sandbox/#install)  
    2. Get [Hortonworks Sandbox v 2.2.4](http://hortonworks.com/products/hortonworks-sandbox/#archive)

* **Dependent services**

    1. Grant root as HBase superuser
    ![add superuser](/images/docs/hbaseSuperuser.png)

    2. Start Storm, HBase & Kafka via Ambari UI. Showing Storm as an example below.
    ![Restart Services](/images/docs/startStorm.png "Services")

### Eagle Installation Steps

* **Step 1**: Download Eagle tarball

    * **Option 1**: Download eagle jar from [here](http://xyz.com).

    * **Option 2**: Build form source code [eagle github](https://github.com/eBay/Eagle), and eagle-xxx-bin.tar.gz will be generated under `./eagle-assembly/target`

          mvn clean install -DskipTests=true

* **Step 2**: Copy the tarball into sandbox and extract it

      #extract
      tar -zxvf eagle-0.1.0-bin.tar.gz
      mv eagle-0.1.0 /usr/hdp/current/eagle

* **Step 3**: Install Eagle service and three monitoring topologies, including HdfsAuditLog, HiveQueryLog, and [OnlineUserProfiles](/docs/onlineUserProfiles.html)

    * **Option 1**: Start Eagle Service using command line

          cd /usr/hdp/current/eagle
          examples/eagle-sandbox-starter.sh

    * **Option 2**: Start Eagle Service using [Eagle Ambari plugin](/docs/ambariPluginInstall.html)

* **Step 4**: Check [Eagle service UI](http://localhost:9099/eagle-service) and [topology UI](http://localhost:8744) with login account `admin/secret`.
(If the network is NAT in virtual box, it's necessary to add service port 9099 to the forwarding port)
![Forwarding Port](/images/docs/eagleService.png)

* **Step 5**: (Optional) To enable the alerting function of HDFSAuditLog, a log4j Kafka appender need to be installed to stream audit log into Kafka. Another option Logstash is [here](/docs/importHDFSAuditLog.html).

    1. Configure Advanced hadoop-log4j via <a href="http://localhost:8080/#/main/services/HDFS/configs" target="_blank">Ambari UI</a>, and add a log4j appender called "KAFKA_HDFS_AUDIT" to hdfs audit logging.

            log4j.appender.KAFKA_HDFS_AUDIT=eagle.log4j.kafka.KafkaLog4jAppender
            log4j.appender.KAFKA_HDFS_AUDIT.Topic=sandbox_hdfs_audit_log
            log4j.appender.KAFKA_HDFS_AUDIT.BrokerList=sandbox.hortonworks.com:6667
            log4j.appender.KAFKA_HDFS_AUDIT.KeyClass=eagle.log4j.kafka.hadoop.AuditLogKeyer
            log4j.appender.KAFKA_HDFS_AUDIT.Layout=org.apache.log4j.PatternLayout
            log4j.appender.KAFKA_HDFS_AUDIT.Layout.ConversionPattern=%d{ISO8601} %p %c{2}: %m%n
            log4j.appender.KAFKA_HDFS_AUDIT.ProducerType=async
            #log4j.appender.KAFKA_HDFS_AUDIT.BatchSize=1
            #log4j.appender.KAFKA_HDFS_AUDIT.QueueSize=1

    2. Edit Advanced hadoop-env via <a href="http://localhost:8080/#/main/services/HDFS/configs" target="_blank">Ambari UI</a>, and add the reference to KAFKA_HDFS_AUDIT to HADOOP_NAMENODE_OPTS.

            -Dhdfs.audit.logger=INFO,DRFAAUDIT,KAFKA_HDFS_AUDIT

    3. Edit Advanced hadoop-env via <a href="http://localhost:8080/#/main/services/HDFS/configs" target="_blank">Ambari UI</a>, and append the following command to it.

            export HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/usr/hdp/current/eagle/lib/log4jkafka/lib/*

    4. Save the changes and restart the namenode
    5. Check whether logs are flowing into Topic `sandbox_hdfs_audit_log` when Kafka is started

            /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic sandbox_hdfs_audit_log


You have now successfully installed Eagle. You can try creating new policies on HDFS and Hive data sets and generate alerts.

### Demo on Alerting

* HDFS Audit log

      $ hdfs dfs -cat /tmp/private

* Hive Query Log

      $ su hive
      $ hive
      > set hive.execution.engine=mr;
      > use xademo;
      > select a.phone_number from customer_details a, call_detail_records b where a.phone_number=b.phone_number;

### Stop Services

* Stop eagle service

      bin/eagle-service.sh stop

* Stop eagle topologies

      bin/eagle-topology.sh --topology sandbox-hdfsAuditLog-topology stop
      bin/eagle-topology.sh --topology sandbox-hiveQueryRunning-topology stop
      bin/eagle-topology.sh --topology sandbox-userprofile-topology stop

