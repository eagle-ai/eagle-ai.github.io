---
layout: doc
title:  "Deploying Eagle in Sandbox"
permalink: /docs/deploymentInSandbox.html
---

### Pre-requisites

##### Environment
> To install eagle on a sandbox you need a HDP sandbox image imported in a virtual machine with 8GB memory recommended.
>
> 1. [Virtualization environment](http://hortonworks.com/products/hortonworks-sandbox/#install) two options
> 2. [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox/#install) v 2.2.4 or later.

##### Register HDP sandbox
> 1. [Register](http://127.0.0.1:8888/) Hortonworks sandbox.
> 2. [Enable Ambari](http://127.0.0.1:8000/). Click on Enable Button.
> 3. [Login](http://127.0.0.1:8080) as admin/admin.

##### Start Storm & HBase & Kafka via Ambari
> 1. Add root as a HBase superuser via [Ambari](http://127.0.0.1:8080/#/main/services/HBASE/configs) (Optional, a user can operate HBase by sudo su hbase, as an alternative).
>
    ![Hbase User](/images/docs/AmbariHbaseConfig.png "Hbase User")
> 2. Start HBase & Storm & Kafka from Ambari UI. Here using Storm as an example
![Restart Services](/images/docs/startStorm.png "Services")

##### Prepare the audit log data for Eagle (Only for HDFSAuditLog Monitoring)
> 1. Make sure a Kafka topic has been created in which Eagle reads the data.
>
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic sandbox_hdfs_audit_log
> 2. Please refer to [here](/docs/importHDFSAuditLog.html) on how to populate log data into kafka.

### Steps of Eagle Installation

Step 1: Download Eagle tarball

* Option 1: build form source code [eagle github](https://github.xyz.com/eagle/eagle/tree/release1.0), and eagle-xxx-bin.tar.gz will be generated under ./eagle-assembly/target

        mvn clean install -DskipTests=true

* Option 2: download the successful built package through the Internet.

Step 2: Extract Eagle tarball package

        # copy the package into sandbox
        scp -P 2222 eagle-0.1.0-bin.tar.gz root@127.0.0.1:/usr/hdp/current/.
        # extract the package
        cd /usr/hdp/current
        tar -zxvf eagle-0.1.0-bin.tar.gz
        mv eagle-0.1.0 eagle

Step 3: Install Eagle service and three monitoring topologies, including HdfsAuditLog, HiveQueryLog, and [OnlineUserProfiles](/docs/onlineUserProfiles.html)

* Option 1: start with eagle command line

        example/eagle-sandbox-starter.sh

* Option 2: start with eagle Ambari plugin

    1. `/usr/hdp/current/eagle/bin/eagle-ambari.sh`

    2. Restart [Ambari](http://127.0.0.1:8000/) click on disable and enable Ambari back.

    3. Add Eagle Service to Ambari. Click on "Add Service" on Ambari Main page

        ![AddService](/images/docs/AddService.png "AddService")
        ![Eagle Services](/images/docs/EagleServiceSuccess.png "Eagle Services")

    4. Add Policies and meta data required by running below script.

            cd eagle
            examples/sample-sensitivity-resource-create.sh
            examples/sample-policy-create.sh

Now Eagle is installed and Here is [Eagle service UI](http://sandbox.hortonworks.com:9099/eagle-service) and [topology UI](http://sandbox.hortonworks.com:8744).

### **Q & A**

Q1. Not able to access storm worker log via browser

A1: add the following line in host machine's hosts file

      127.0.0.1 sandbox.hortonworks.com

Q2. Not able send data into kafka using kafka console producer: /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list localhost:6667 --topic sandbox_hdfs_audit_log

A2: kafka broker are binding to host sandbox.hortonworks.com

       /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list sandbox.hortonworks.com:6667 --topic sandbox_hdfs_audit_log
       cat /etc/hosts
       127.0.0.1 localhost.localdomain localhost
       10.0.2.15 sandbox.hortonworks.com sandbox ambari.hortonworks.com

Q3. Cannot open eagle service url localhost:9099

A3: If your network is NAT, then you need to add forwarding port 9099 for eagle service

