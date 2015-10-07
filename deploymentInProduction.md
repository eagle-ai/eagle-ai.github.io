---
layout: doc
title:  "Deploying Eagle in the Production"
permalink: /docs/deploymentInProduction.html
---


### **Pre-requisites**

##### Environment
> * HBase: 0.98 or later
> * Storm: 0.9.3 or later
> * Kafka: 0.8.x or later
> * Java: 1.7.x

##### Prepare the audit log data for Eagle (Only for HDFSAuditLog Monitoring)
1. Make sure a Kafka topic has been created in which Eagle reads the data.
2. Please refer to [here](/docs/importHDFSAuditLog.html) on how to populate log data into kafka.


### **Steps of installing Eagle**

Step 0: prepare the data, please refer to [here](/docs/importHDFSAuditLog.html) if you want to enable HDFS audit log monitoring.

Step 1: edit configuration files

* edit bin/eagle-env.sh

        #TODO: make sure java version is 1.7.x
        export JAVA_HOME=

        # nimbus.host. Default is localhost
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

* create a configuration file for a eagle topology under conf/, and please refer to the sample versions

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

        # Here is an example
        #bin/eagle-topology.sh --jar lib/topology/eagle-topology-0.1.0-assembly.jar --main com.ebay.eagle.security.auditlog.HdfsAuditLogProcessorMain --config conf/apollo-phx-hdfsAuditLog-application.conf start





