---
layout: doc
title:  "Deploy Eagle in the Production"
permalink: /docs/deploymentInProduction.html
---


Eagle requires you to have access on Hadoop CLI, where you have full permissions to HDFS, Storm, HBase and Kafka. To make things easier, we strongly recommend you to [start Eagle on a sandbox](/docs/deploymentInSandbox.html).


### **Pre-requisites**

##### Environment
> * HBase: 0.98 or later
> * Storm: 0.9.3 or later
> * Kafka: 0.8.x or later
> * Java: 1.7.x


##### Prepare the audit log data for Eagle (Only for HDFSAuditLog Monitoring)
> 1. Create a Kafka topic for importing audit log. Here is an example command to create topic named sandbox_hdfs_audit_log.
>
         cd <kafka-home>
         bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic sandbox_hdfs_audit_log
> 2. Populate audit log into the Kafka topic created above, and refer to [here](/docs/importHDFSAuditLog.html) on How to do it.


### **Steps of installing Eagle**

Step 1: edit configuration files

* edit bin/eagle-env.sh

        # TODO: make sure java version is 1.7.x
        export JAVA_HOME=

        # TODO: Storm nimbus host. Default is localhost
        export EAGLE_NIMBUS_HOST=localhost

        # TODO: EAGLE_SERVICE_HOST, default is `hostname -f`
        export EAGLE_SERVICE_HOST=`hostname -f`

        # TODO: EAGLE_SERVICE_PORT, default is 9099
        export EAGLE_SERVICE_PORT=9099


* edit conf/eagle-service.conf

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

* create a configuration file for each type of Eagle topology under conf/, and please refer to the [sample versions](https://github.xyz.com/eagle/eagle/tree/master/eagle-assembly/src/main/conf).

        # Here are some importance configurations.

        # TODO: change mode to cluster
        "mode" : "cluster"

        # TODO: make sure the kafka topic same as your topic that have been created
        "topic" : "sandbox_hdfs_audit_log",

        # TODO: update site
        "site" : "sandbox",

        # Eagle service host
        "eagle.service.host" : "localhost",

        # Eagle service port
        "eagleServicePort" : 9099,

        # SMTP server
        "mail.host" : "mx.xyz.com",
        "mail.smtp.port":"25",


Step 2: Start Eagle services

* start eagle service

        su eagle
        cd <eagle-home>

        # create HBase tables for the first installation
        bin/eagle-service-init.sh

        # start eagle service
        bin/eagle-service.sh start

* start an eagle topology

        bin/eagle-topology-init.sh

        # start eagle topology
        bin/eagle-topology.sh --jar <topologyJar> --main <mainClass> --config <path-to-config> start

  Here are some examples

        # start HDFS audilt log monitoring
        bin/eagle-topology.sh --jar lib/topology/eagle-topology-0.1.0-assembly.jar --main eagle.security.auditlog.HdfsAuditLogProcessorMain --config conf/apollo-phx-hdfsAuditLog-application.conf start

        # start Hive Query Log Monitoring
        bin/eagle-topology.sh --main eagle.security.hive.jobrunning.HiveJobRunningMonitoringMain --config ${EAGLE_HOME}/conf/sandbox-hiveQueryLog-application.conf start

        # start User Profiles
        bin/eagle-topology.sh --main eagle.security.userprofile.UserProfileDetectionMain --config conf/sandbox-userprofile-topology.conf --topology userprofile-topology start

Step 3: Check [Eagle service UI](http://sandbox.hortonworks.com:9099/eagle-service) and [topology UI](http://sandbox.hortonworks.com:8744) with login account `admin/secret`.


