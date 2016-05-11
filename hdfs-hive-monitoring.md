---
layout: doc
title:  "HDFS & Hive Quick Start" 
permalink: /docs/hdfs-hive-monitoring.html
---

This Guide describes the steps to install data activity monitoring of "HDFS File System" & "Hive".

* Prerequisite
* Stream HDFS audit logs into Kafka.
* Demos "HDFS File System" & "Hive" monitoring
<br/><br/>


### **Prerequisite**
* Complete the setup from [Quick Starter(Eagle In Sandbox)](/docs/quick-start.html)	 	
<br/><br/>


### **Stream HDFS audit logs into Kafka**   
 
  Note: This section is only needed for "HDFS File System" Monitoring. For another option to stream HDFS audit logs into Kafka using Logstash [Click Here](/docs/import-hdfs-auditLog.html)
 
* **Step 1**: Configure Advanced hadoop-log4j via <a href="http://localhost:8080/#/main/services/HDFS/configs" target="_blank">Ambari UI</a>, by adding below "KAFKA_HDFS_AUDIT" log4j appender to hdfs audit logging.

	   log4j.appender.KAFKA_HDFS_AUDIT=org.apache.eagle.log4j.kafka.KafkaLog4jAppender
	   log4j.appender.KAFKA_HDFS_AUDIT.Topic=sandbox_hdfs_audit_log
	   log4j.appender.KAFKA_HDFS_AUDIT.BrokerList=sandbox.hortonworks.com:6667
	   log4j.appender.KAFKA_HDFS_AUDIT.KeyClass=org.apache.eagle.log4j.kafka.hadoop.AuditLogKeyer
	   log4j.appender.KAFKA_HDFS_AUDIT.Layout=org.apache.log4j.PatternLayout
	   log4j.appender.KAFKA_HDFS_AUDIT.Layout.ConversionPattern=%d{ISO8601} %p %c{2}: %m%n
	   log4j.appender.KAFKA_HDFS_AUDIT.ProducerType=async

    ![HDFS LOG4J Configuration](/images/docs/hdfs-log4j-conf.png "hdfslog4jconf")

* **Step 2**: Edit Advanced hadoop-env via <a href="http://localhost:8080/#/main/services/HDFS/configs" target="_blank">Ambari UI</a>, and add the reference to KAFKA_HDFS_AUDIT to HADOOP_NAMENODE_OPTS.

      -Dhdfs.audit.logger=INFO,DRFAAUDIT,KAFKA_HDFS_AUDIT

    ![HDFS Environment Configuration](/images/docs/hdfs-env-conf.png "hdfsenvconf")

* **Step 3**: Edit Advanced hadoop-env via <a href="http://localhost:8080/#/main/services/HDFS/configs" target="_blank">Ambari UI</a>, and append the following command to it.

      export HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/usr/hdp/current/eagle/lib/log4jkafka/lib/*

    ![HDFS Environment Configuration](/images/docs/hdfs-env-conf2.png "hdfsenvconf2")

* **Step 4**: save the changes 

* **Step 5**: "Restart All" Storm & Kafka from Ambari.

* **Step 6**: Restart name node 

![Restart Services](/images/docs/nn-restart.png "Services")

* **Step 7**: Check whether logs from "/var/log/hadoop/hdfs/hdfs-audit.log" are flowing into topic `sandbox_hdfs_audit_log`
      
        $ /usr/hdp/2.2.4.2-2/kafka/bin/kafka-console-consumer.sh --zookeeper sandbox.hortonworks.com:2181 --topic sandbox_hdfs_audit_log      
      
<br/>


### **Demos**
* Login to Eagle UI [http://localhost:9099/eagle-service/](http://localhost:9099/eagle-service/) using username and password as "admin" and "secret"
* **HDFS**:
	1. Click on menu "DAM" and select "HDFS" to view HDFS policy
	2. You should see policy with name "viewPrivate". This Policy generates alert when any user reads HDFS file name "private" under "tmp" folder.
	3. In sandbox read restricted HDFS file "/tmp/private" by using command 
	
	   > hadoop fs -cat /tmp/private

	From UI click on alert tab and you should see alert for the attempt to read restricted file.  
* **Hive**:
	1. Click on menu "DAM" and select "Hive" to view Hive policy
	2. You should see policy with name "queryPhoneNumber". This Policy generates alert when hive table with sensitivity(Phone_Number) information is queried. 
	3. In sandbox read restricted sensitive HIVE column. ( To learn more about data sensitivity settings click [Data Classification Tutorial](/docs/tutorial/classification.html))
	
        $ su hive <br/>
        $ hive <br/>
        $ set hive.execution.engine=mr; <br/>
        $ use xademo; <br/>
        $ select a.phone_number from customer_details a, call_detail_records b where a.phone_number=b.phone_number; <br/>

    From UI click on alert tab and you should see alert for your attempt to dfsf read restricted column.  
<br/>
