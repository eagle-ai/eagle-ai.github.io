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

    2. Populate audit log into the Kafka topic created above, and refer to [here](/docs/import-hdfs-auditLog.html) on How to do it.


### **Install Steps**

* **Edit environment related configurations**:

    * `bin/eagle-env.sh`

            # TODO: make sure java version is 1.7.x
            export JAVA_HOME=

            # TODO: Storm nimbus host. Default is localhost
            export EAGLE_NIMBUS_HOST=localhost

            # TODO: EAGLE_SERVICE_HOST, default is `hostname -f`
            export EAGLE_SERVICE_HOST=localhost


    * Edit `conf/eagle-service.conf`

            # TODO: hbase.zookeeper.quorum in the format host1,host2,host3,...
            # default is "localhost"
            hbase-zookeeper-quorum="localhost"

            # TODO: hbase.zookeeper.property.clientPort
            # default is 2181
            hbase-zookeeper-property-clientPort=2181

            # TODO: hbase configuration: zookeeper.znode.parent
            # default is "/hbase"
            zookeeper-znode-parent="/hbase"

    * **Edit topology related configurations**

      Create a configuration file for each type of Eagle topologies under conf/, and here are [sample versions](https://github.com/eBay/Eagle/tree/master/eagle-assembly/src/main/conf).
      and [topology configuration description](/docs/configuration.html)


* **Install and start Eagle services**

    * Install Eagle

          $ su eagle
          $ cd <eagle-home>

          # create HBase tables
          $ bin/eagle-service-init.sh

          # start Eagle service
          $ bin/eagle-service.sh start

          # import metadata after Eagle service is successfully started
          $ bin/eagle-topology-init.sh

    * Start eagle topologies

          # start HDFS audilt log monitoring (Optional)
          $ bin/eagle-topology.sh --main eagle.security.auditlog.HdfsAuditLogProcessorMain --config conf/your-hdfsAuditLog-application.conf start

          # start Hive Query Log Monitoring (Optional)
          $ bin/eagle-topology.sh --main eagle.security.hive.jobrunning.HiveJobRunningMonitoringMain --config conf/your-hiveQueryLog-application.conf start

          # start User Profiles (Optional)
          $ bin/eagle-topology.sh --main eagle.security.userprofile.UserProfileDetectionMain --config conf/your-userprofile-topology.conf start


You have now successfully installed Eagle and started Eagle Services. You can

* visit [Eagle web] on http://localhost:9099/eagle-service with username/password `admin/secret`, and check topologies on Storm UI

* try a simple demo [Quick Starer](/docs/quick-start.html).

### **Stop Services**

* Stop eagle service

      $ bin/eagle-service.sh stop

* Stop eagle topologies

      $ bin/eagle-topology.sh --topology custom-hdfsAuditLog-topology stop
      $ bin/eagle-topology.sh --topology custom-hiveQueryRunning-topology stop
      $ bin/eagle-topology.sh --topology custom-userprofile-topology stop
