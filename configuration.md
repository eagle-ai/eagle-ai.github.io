---
layout: doc
title:  "Eagle Topology Configuration"
permalink: /docs/configuration.html
---

Eagle requires you to create a configuration file under `conf/` directory for each topology first. This page will give detailed
description of Eagle topology configuration.

The typical topology configuration file in Eagle consists of five parts:

* envContextConfig
* dataSourceConfig
* alertExecutorConfigs
* eagleProps
* dynamicConfigSource

### **envContextConfig**

* Common

    Property Name               | Default             | Description
    --------------------------- | ------------------- | ------------
    env                         | Content Cell        | Content Cell
    mode                        | Content Cell        | Content Cell
    topologyName                | Content Cell        | Content Cell
    stormConfigFile             | Content Cell        | Content Cell
    kafkaMsgConsumer            | Content Cell        | Content Cell
    hdfsAuditLogAlertExecutor   | Content Cell        | Content Cell

### **dataSourceConfig**

* HDFS

    Property Name               | Default                                               | Description
    --------------------------- | ----------------------------------------------------  | ------------
    topic                       | sandbox_hdfs_audit_log                                | Content Cell
    zkConnection                | 127.0.0.1:2181                                        | Content Cell
    zkConnectionTimeoutMS       | 15000                                                 | Content Cell
    fetchSize                   | 1048586                                               | Content Cell
    deserializerClass           | HdfsAuditLogKafkaDeserializer                         | Content Cell
    transactionZKServers        | 127.0.0.1                                             | Content Cell
    transactionZKPort           | 2181                                                  | Content Cell
    transactionZKRoot           | /consumers                                            | Content Cell
    consumerGroupId             | eagle.hdfsaudit.consumer                              | Content Cell
    transactionStateUpdateMS    | 2000                                                  | Content Cell

* HIVE

    Property Name               | Default                             | Description
    --------------------------- | ----------------------------------- | ------------
    flavor                      | stormrunning                        | Content Cell
    zkQuorum                    | localhost:2181                      | Content Cell
    zkRoot                      | /jobrunning                         | Content Cell
    zkSessionTimeoutMs          | 15000                               | Content Cell
    zkRetryTimes                | 3                                   | Content Cell
    zkRetryInterval             | 2000                                | Content Cell
    RMEndPoints                 | http://localhost:8088/              | Content Cell
    HSEndPoint                  | http://localhost:19888/             | Content Cell
    partitionerCls              | eagle.job.DefaultJobPartitionerImpl | Content Cell

### **alertExecutorConfigs**

* Common

    Property Name               | Default             | Description
    --------------------------- | ------------------- | ------------
    parallelism                 | 1                 | Content Cell
    partitioner                 | eagle.alert.policy.DefaultPolicyPartitioner       | Content Cell
    needValidation              | true        | Content Cell

### **eagleProps**

* Common

    Property Name               | Default             | Description
    --------------------------- | ------------------- | ------------
    site                        | sandbox        | Content Cell
    dataSource                 | hdfsAuditLog        | Content Cell
    dataJoinPollIntervalSec                | 30        | Content Cell
    mailHost                    |         | Content Cell
    mailSmtpPort            | 25       | Content Cell
    mailDebug   | true       | Content Cell
    eagleService.host                | localhost        | Content Cell
    eagleService.port                    |  9099       | Content Cell
    eagleService.username            | admin       | Content Cell
    eagleService.password      | secret       | Content Cell

### **dynamicConfigSource**

* Common

    Property Name               | Default             | Description
    --------------------------- | ------------------- | ------------
    enabled                     | true       | Content Cell
    initDelayMillis             | 0       | Content Cell
    delayMillis                 | 30000        | Content Cell
