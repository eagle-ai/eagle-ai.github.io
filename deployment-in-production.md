---
layout: doc
title:  "Deploy Eagle in the Production"
permalink: /docs/deployment-in-production.html
---


This page outlines the steps for manually deploying Eagle in the production environment. For a quick start, we strongly recommend you to [start Eagle in a sandbox](/docs/deployment-in-sandbox.html).


Here's the main content of this page:

* Pre-requisites
   * Environment
   * Stream HDFS audit log data into Kafka
* Install Steps
   * Edit Configure files
   * Install and start Eagle services
* Stop Services


### **Pre-requisites**

* **Environment**

    * HBase: 0.98 or later
    * Storm: 0.9.3 or later
    * Kafka: 0.8.x or later
    * Java: 1.7.x

* **Stream HDFS audit log data (Only for HDFSAuditLog Monitoring)**

    1. Create a Kafka topic for importing audit log. Here is an example command to create topic sandbox_hdfs_audit_log.

           $ cd <kafka-home>
           $ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic sandbox_hdfs_audit_log

    2. Populate audit log into the Kafka topic created above, and refer to [here](/docs/importHDFSAuditLog.html) on How to do it.


### **Install Steps**

* **Edit configuration files**:

    * Edit `bin/eagle-env.sh`

          # TODO: make sure java version is 1.7.x
          export JAVA_HOME=

          # TODO: Storm nimbus host. Default is localhost
          export EAGLE_NIMBUS_HOST=localhost

          # TODO: EAGLE_SERVICE_HOST, default is `hostname -f`
          export EAGLE_SERVICE_HOST=localhost

          # TODO: EAGLE_SERVICE_PORT, default is 9099
          export EAGLE_SERVICE_PORT=9099

    * Edit `conf/eagle-service.conf`

          # storage type: ["hbase","jdbc"]
          # default is "hbase"
          storage-type="hbase"

          # hbase configuration: hbase.zookeeper.quorum, the format is as host1,host2,host3,...
          # default is "localhost"
          hbase-zookeeper-quorum="localhost"

          # hbase configuration: hbase.zookeeper.property.clientPort
          # default is 2181
          hbase-zookeeper-property-clientPort=2181

          # hbase configuration: zookeeper.znode.parent
          # default is "/hbase"
          zookeeper-znode-parent="/hbase"

    * Create a configuration file for each type of Eagle topology under conf/, and please refer to the [sample versions](https://github.com/eBay/Eagle/tree/master/eagle-assembly/src/main/conf).
      For more detailed information on configuration, please refer to [this page](/docs/configuration.html)


* **Install and start Eagle services**

    * Start eagle service

          $ su eagle
          $ cd <eagle-home>

          # create HBase tables for the first installation
          $ bin/eagle-service-init.sh

          # start eagle service
          $ bin/eagle-service.sh start

    * Start eagle topologies

          # import metadata for the first installation
          $ bin/eagle-topology-init.sh

          # start HDFS audilt log monitoring (Optional)
          $ bin/eagle-topology.sh --main eagle.security.auditlog.HdfsAuditLogProcessorMain --config conf/custom-hdfsAuditLog-application.conf start

          # start Hive Query Log Monitoring (Optional)
          $ bin/eagle-topology.sh --main eagle.security.hive.jobrunning.HiveJobRunningMonitoringMain --config conf/custom-hiveQueryLog-application.conf start

          # start User Profiles (Optional)
          $ bin/eagle-topology.sh --main eagle.security.userprofile.UserProfileDetectionMain --config conf/custom-userprofile-topology.conf start


You have now successfully installed Eagle. Now you can

* visit [Eagle web](http://localhost:9099/eagle-service) with username/password `admin/secret`, and check
[Storm topologies](http://localhost:8744).
* try a simple demo on [Quick Starer](/docs/quick-start.html).

### **Stop Services**

* Stop eagle service

      $ bin/eagle-service.sh stop

* Stop eagle topologies

      $ bin/eagle-topology.sh --topology custom-hdfsAuditLog-topology stop
      $ bin/eagle-topology.sh --topology custom-hiveQueryRunning-topology stop
      $ bin/eagle-topology.sh --topology custom-userprofile-topology stop
