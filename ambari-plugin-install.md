---
layout: doc
title:  "Install Apache Eagle (incubating) Ambari Plugin"
permalink: /docs/ambari-plugin-install.html
---

Assume Apache Eagle (incubating, called Eagle in the following) package has been copied and exacted under /usr/hdp/current/eagle.

> **WARNING**: the following instructions work in sandbox currently.


### Pre-requisites

1. Create a Kafka[^KAFKA] topic if you have not. Here is an example command.

       $ /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic sandbox_hdfs_audit_log

2. Stream HDFS log data to Kafka, and refer to [here](/docs/import-hdfs-auditLog.html) on how to do it .

### Installation Steps

1. Start dependent services Storm[^STORM], Spark[^SPARK], HBase[^HBASE] & Kafka via Ambari[^AMBARI].

2. Install Eagle Ambari plugin

       $ /usr/hdp/current/eagle/bin/eagle-ambari.sh install

3. Restart [Ambari](http://127.0.0.1:8000/) click on disable and enable Ambari back.

4. Add Eagle Service to Ambari. Click on "Add Service" on Ambari Main page

    ![AddService](/images/docs/add-service.png "AddService")
    ![Eagle Services](/images/docs/eagle-service-success.png "Eagle Services")

5. Add Policies and meta data required by running the below script.

       $ cd <eagle-home>
       $ examples/sample-sensitivity-resource-create.sh
       $ examples/sample-policy-create.sh



---

#### *Footnotes*

[^HBASE]:*Apache HBase.*
[^AMBARI]:*All mentions of "ambari" on this page represent Apache Ambari.*
[^SPARK]:*Apache Spark.*
[^KAFKA]:*All mentions of "kafka" on this page represent Apache Kafka.*
[^STORM]:*Apache Storm.*
