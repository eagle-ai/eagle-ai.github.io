---
layout: doc
title:  "Application Configuration"
permalink: /docs/configuration.html
---

Apache Eagle (incubating, called Eagle in the following) requires you to create a configuration file under `$EAGLE_HOME/conf/` for each application. Basically, there are some common properties shared, e.g., envContextConfig, eagleProps, and dynamicConfigSource. While dataSourceConfig differs from application to application.

In this page we take the following two application as examples

* HDFS Audit Log Configuration
* Hive Query Log Configuration


### HDFS Audit Log Configuration
---

 Class            ||| Property Name    ||| Description
 -----------------||| -------------    ||| -----------
 envContextConfig |||   env            ||| currently only Storm is supported.
                  |||   mode           ||| local or cluster
                  |||   topologyName   ||| in the format {site}-{topology-name}
                  |||   stormConfigFile    ||| a storm configuration file for overriding some Storm properties
                  |||  parallelismConfig   ||| parallelism for both kafka consumer and alert executors
dataSourceConfig  |||  **topic**           ||| Kafka topic for audit log streaming, make sure it exists
                  ||| **zkConnection***    |||ZooKeeper connection string, you can also specify multiple hosts in the form hostname1:port1,hostname2:port2, ...
                  |||zkConnectionTimeoutMS ||| zookeeper connection timeout
                  |||   fetchSize          ||| kafka maximal message fetching size, default value is 1048586
                  |||   deserializerClass  ||| org.apache.eagle.security.auditlog.HdfsAuditLogKafkaDeserializer 
                  ||| transactionZKServers ||| ZooKeeper servers, you can also specify multiple hosts in the form hostname1,hostname2,hostname3
                  ||| transactionZKPort    ||| ZooKeeper connection port
                  |||   transactionZKRoot  ||| ZooKeeper chroot path for Eagle
                  ||| consumerGroupId      ||| only for hdfsAuditLog
                  ||| transactionStateUpdateMS   ||| default is 2000
alertExecutorConfigs ||| parallelism             ||| default is 1
                  |||   partitioner              ||| default value is eagle.alert.policy.DefaultPolicyPartitioner
                  |||   needValidation           ||| true or false
eagleProps        |||   **site***                ||| site name, such as sandbox, datacenter1, datacenter2
                  |||   dataSource               ||| hdfsAuditLog
                  |||   dataJoinPollIntervalSec  ||| time interval for retrieving data from HBase
                  |||   **mailHost***                 ||| SMTP server
                  |||   **mailSmtpPort***             ||| SMTP server port, default is 25
                  |||   mailDebug                ||| true or false
                  |||   eagleService.host        ||| tomcat server host, default is localhost
                  |||   eagleService.port        ||| 9099
                  |||   eagleService.username    ||| admin
                  |||   eagleService.password    ||| secret
 dynamicConfigSource ||| enabled                 ||| true or false, default is true
                     |||   initDelayMillis       ||| default is 0
                     |||   delayMillis           ||| default is 30000


<br />

### Hive Query Log Configuration
---

 Class            ||| Property Name           ||| Description
 -----------------||| -------------           ||| -----------
 envContextConfig |||  same as HDF            |||
 dataSourceConfig |||  flavor                 ||| stormrunning
 |||   **zkQuorum***                               ||| ZooKeeper connection string,  you can also specify multiple hosts in the form hostname1:port1,hostname2:port2,hostname3:port3
 |||   **zkRoot***                                 ||| ZooKeeper chroot path for Eagle to store data, default is /jobrunning
 |||   zkSessionTimeoutMs                     ||| ZooKeeper session timeout, default is 15000
 |||   zkRetryTimes                           ||| ZooKeeper retry times, default is 3
 |||   zkRetryInterval                        ||| default is 2000
 |||   **RMEndPoints***                       ||| yarn.resourcemanager.webapp.address. Default value is http://localhost:8088/
 |||   **HSEndPoint***                        ||| mapreduce.jobhistory.webapp.address. Default values is http://localhost:19888/
 |||   partitionerCls                         ||| eagle.job.DefaultJobPartitionerImpl
 alertExecutorConfigs ||| same as HDFS        |||
 eagleProps           ||| same as HDFS        |||
 dynamicConfigSource  ||| same as HDFS        |||

<br />
