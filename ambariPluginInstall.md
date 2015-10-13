---
layout: doc
title:  "Install Eagle Ambari Plugin"
permalink: /docs/ambariPluginInstall.html
---

Assume Eagle package has been copied and exacted under /usr/hdp/current/eagle.


### Pre-requisites

##### Prepare the audit log data for Eagle (For HDFS)
> 1. Create a Kafka topic if you have not. Here is an example command.
>
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic sandbox_hdfs_audit_log
> 2. Populate log data to the kafka topic, and refer to [here](/docs/importHDFSAuditLog.html) on how to do it .


###Steps

1. Start dependent services Storm, Spark, HBase & Kafka via Ambari.

2. `/usr/hdp/current/eagle/bin/eagle-ambari.sh install`

3. Restart [Ambari](http://127.0.0.1:8000/) click on disable and enable Ambari back.

4. Add Eagle Service to Ambari. Click on "Add Service" on Ambari Main page

    ![AddService](/images/docs/AddService.png "AddService")
    ![Eagle Services](/images/docs/EagleServiceSuccess.png "Eagle Services")

5. Add Policies and meta data required by running the below script.

        cd <eagle-home>
        examples/sample-sensitivity-resource-create.sh
        examples/sample-policy-create.sh