---
layout: doc
title:  "Deploy Eagle in Sandbox"
permalink: /docs/deployment-in-sandbox.html
---

### Pre-requisites

* **Environment**

    To install eagle on a sandbox you need to run a HDP sandbox image in a virtual machine with 8GB memory recommended.

    1. Get Virtual Box or VMware [Virtualization environment](http://hortonworks.com/products/hortonworks-sandbox/#install)  
    2. Get [Hortonworks Sandbox v 2.2.4](http://hortonworks.com/products/hortonworks-sandbox/#archive)

* **Dependent services**

    1. Launch Ambari to manage the Hadoop environment in sandbox
       * Enable Ambari in sandbox http://127.0.0.1:8000 (Click on Enable Button)
       * Login to Ambari UI http://127.0.0.1:8080/ with username and password as "admin"
    2. Grant root as HBase superuser via Ambari
    ![add superuser](/images/docs/hbase-superuser.png)
    3. Start Storm, HBase & Kafka Ambari. Showing Storm as an example below.
    ![Restart Services](/images/docs/start-storm.png "Services")

### Eagle Installation Steps

* **Step 1**: Download Eagle tarball

    * **Option 1**: Download eagle jar from [here](http://66.211.190.194/eagle-0.1.0.tar.gz).

    * **Option 2**: Build form source code [eagle github](https://github.com/eBay/Eagle). After successful build, ‘eagle-xxx-bin.tar.gz’ will be generated under `./eagle-assembly/target`
          
          $ mvn clean install -DskipTests=true
          
* **Step 2**: Copy the tarball into sandbox and extract it

      #extract
      $ tar -zxvf eagle-0.1.0-bin.tar.gz
      $ mv eagle-0.1.0 /usr/hdp/current/eagle

* **Step 3**: Install Eagle service and three monitoring topologies, including HdfsAuditLog, HiveQueryLog, and [OnlineUserProfiles](/docs/online-user-profiles.html)

    * **Option 1**: Start Eagle Service using command line

          $ cd /usr/hdp/current/eagle
          $ examples/eagle-sandbox-starter.sh

    * **Option 2**: Start Eagle Service using [Eagle Ambari plugin](/docs/ambari-plugin-install.html)

* **Step 4**: Check [Eagle service UI](http://localhost:9099/eagle-service) and [topology UI](http://localhost:8744) with login account `admin/secret`.
(If the network is NAT in virtual box, it's necessary to add service port 9099 to the forwarding port)
![Forwarding Port](/images/docs/eagle-service.png)


You have now successfully installed Eagle. You can try creating new policies on HDFS and Hive data sets and generate alerts.

### Demo on Alerting

**Example 1** (HDFSAuditLog): check sample policy “viewPrivate” in [Eagle service UI](http://localhost:9099/eagle-service) by importing hdfs audit log into Kafka
topic `sandbox_hdfs_audit_log`

  * **Option 1**: manually import a log into Kafka console producer

        $ /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list sandbox.hortonworks.com:6667 --topic sandbox_hdfs_audit_log
        2015-07-27 20:26:46,881 INFO FSNamesystem.audit: allowed=true ugi=root (auth:SIMPLE) ip=/127.0.0.1 cmd=open src=/tmp/private dst=nul perm=null proto=rpc
  * **Option 2**: install [a namenode log4j Kafka appender](/docs/import-hdfs-auditLog.html) to stream hdfs audit log into Kafka automatically, and then run below command

        # install a log4j Kafka appender first
        $ hdfs dfs -cat /tmp/private

  You should see an alert for policy name “viewPrivate” in [Eagle service UI](http://localhost:9099/eagle-service) . Under Alerts page. 

**Example 2** (HiveQueryLog): check sample policy “queryPhoneNumber” in [Eagle service UI](http://localhost:9099/eagle-service) by submitting a hive job

      $ su hive
      $ hive
      > set hive.execution.engine=mr;
      > use xademo;
      > select a.phone_number from customer_details a, call_detail_records b where a.phone_number=b.phone_number;

  You should see an alert for policy name “queryPhoneNumber” in [Eagle service UI](http://localhost:9099/eagle-service) . Under Alerts page. 

### Stop Services

* Stop eagle service

      $ bin/eagle-service.sh stop

* Stop eagle topologies

      $ bin/eagle-topology.sh --topology sandbox-hdfsAuditLog-topology stop
      $ bin/eagle-topology.sh --topology sandbox-hiveQueryRunning-topology stop
      $ bin/eagle-topology.sh --topology sandbox-userprofile-topology stop

