---
layout: doc
title:  "Configuration"
permalink: /docs/configuration.html
---

Eagle requires you to create a configuration file under `conf/` directory for each topology first. This page will give detailed
description of Eagle topology configuration.

Eagle currently supports to customize configurations for three kinds of topologies:

* HDFS
* HIVE
* UserProfile


#### HdfsAuditLog

 Class            ||| Property Name      ||| Default             ||| Description
 -----------------||| -------------      ||| -------             ||| -----------
 envContextConfig |||   env              ||| storm               ||| currently only Storm support
                  |||   mode             ||| cluster             ||| local or cluster
                  |||   topologyName     ||| sandbox-hdfsAuditLog-topology      ||| format as {site}-{topology-name}, submitted as Storm topology name
                  |||   stormConfigFile            ||| security-auditlog-storm.yaml       ||| ***
                  |||  parallelismConfig          ||| 1                 ||| parallelism for both kafka consumer and alert executors
dataSourceConfig  |||  topic              ||| sandbox_hdfs_audit_log      ||| Kafka topic for audit log streaming, make sure it's created
                  ||| zkConnection        ||| 127.0.0.1:2181              ||| ZooKeeper connection string, you can also specify multiple hosts in the form hostname1:port1,hostname2:port2,hostname3:port3
                  |||zkConnectionTimeoutMS||| 15000                       ||| timeout
                  |||   fetchSize         ||| 1048586                     ||| default value
                  |||   deserializerClass |||                             ||| default value
                  |||transactionZKServers ||| 127.0.0.1                   ||| ZooKeeper servers, you can also specify multiple hosts in the form hostname1,hostname2,hostname3
                  ||| transactionZKPort   ||| 2181                        ||| ZooKeeper connection port
                  |||   transactionZKRoot ||| /consumers                  ||| ZooKeeper chroot path for Eagle
                  ||| consumerGroupId     ||| eagle.hdfsaudit.consumer    ||| default value
                  ||| transactionStateUpdateMS ||| 2000           ||| ****
alertExecutorConfigs ||| parallelism                 ||| 1                   ||| ***
 |||   partitioner                 |||    ||| default value
 |||   needValidation              ||| true        ||| ***
 eagleProps  ||| site                        ||| sandbox        ||| site name, such as sandbox, datacenter1, datacenter2
 |||   dataSource                 ||| hdfsAuditLog        ||| only three data sources hdfsAuditLog, HiveQueryLog, userProfile are supported
 |||   dataJoinPollIntervalSec    ||| 30        ||| time interval for retrieving data from HBase
 |||   mailHost                   |||           ||| SMTP server
 |||   mailSmtpPort               ||| 25        ||| SMTP server port
 |||   mailDebug                 ||| true       ||| ***
 |||   eagleService.host          ||| localhost   |||  tomcat server host
 |||   eagleService.port          |||  9099       ||| tomcat server port
 |||   eagleService.username      ||| admin       ||| default value
 |||   eagleService.password      ||| secret      ||| default value
 dynamicConfigSource ||| enabled  ||| true       ||| ***
 |||   initDelayMillis             ||| 0       |||  ***
 |||   delayMillis                 ||| 30000   ||| ***


#### HiveQueryLog

 Class            ||| Property Name      ||| Default             ||| Description
 -----------------||| -------------      ||| -------             ||| -----------
 envContextConfig |||  same as HDFS      |||                     |||
 dataSourceConfig |||  flavor            ||| stormrunning        ||| ****
 |||   zkQuorum                   ||| localhost:2181                     ||| ZooKeeper connection string,  you can also specify multiple hosts in the form hostname1:port1,hostname2:port2,hostname3:port3
 |||   zkRoot                     ||| /jobrunning                        ||| ZooKeeper chroot path for Eagle to store data
 |||   zkSessionTimeoutMs         ||| 15000                              ||| ZooKeeper session timeout
 |||   zkRetryTimes               ||| 3                                  ||| ZooKeeper retry times
 |||   zkRetryInterval            ||| 2000                               ||| ****
 |||   RMEndPoints                ||| http://localhost:8088/             ||| ****
 |||   HSEndPoint                 ||| http://localhost:19888/            ||| ****
 |||   partitionerCls             |||    ||| eagle.job.DefaultJobPartitionerImpl
 alertExecutorConfigs ||| same as HDFS      |||                     |||
 eagleProps           ||| same as HDFS      |||                     |||
 dynamicConfigSource  ||| same as HDFS      |||                     |||


#### UserProfile