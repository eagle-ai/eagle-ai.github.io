---
layout: doc
title:  "Quick Start" 
permalink: /docs/quick-start.html
---

This is a tutorial-style guide for users to install Eagle, create self-defined policies and checking alert.

### Step 1: Download/Build tarball

* Download tarball directly from latest released [binary package](http://66.211.190.194/eagle-0.1.0.tar.gz)

* Build manaly by cloning latest code from [github](https://github.com/ebay/eagle) with [Apache Maven](https://maven.apache.org/):

	  $ git clone git@github.com:eBay/Eagle.git
	  $ cd Eagle
	  $ mvn clean package -DskipTests

	After building successfully, you will get the tarball under `eagle-assembly/target/` named as `eagle-${version}.tar.gz`
<br/>

### Step 2: Install Eagle
The fastest way to start with Eagle is to:

* [Install Eagle with Sandbox](/docs/deployment-in-sandbox.html)
* [Install Eagle with Docker](https://github.com/eBay/Eagle/issues/2)(under development)

If you want to deploy eagle in production environment, please refer to:

* [Deploy Eagle in the Production](/docs/deployment-in-production.html)
<br/>

### Step 3: Define policy with Eagle web

![](/images/docs/hdfs-policy1.png)

Learn more about how to define policy, please refer to tutorial: [Policy Management](/docs/hdfs-policy.html)
<br/>

### Step 4: Test policy and check alerting

**Example 1** (HDFSAuditLog): check sample policy “viewPrivate” on [Eagle web](http://localhost:9099/eagle-service) by importing hdfs audit log into Kafka
topic `sandbox_hdfs_audit_log`

  * **Option 1**: manually import a sample log into Kafka console producer

        $ /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list sandbox.hortonworks.com:6667 --topic sandbox_hdfs_audit_log
        2015-07-27 20:26:46,881 INFO FSNamesystem.audit: allowed=true ugi=root (auth:SIMPLE) ip=/127.0.0.1 cmd=open src=/tmp/private dst=nul perm=null proto=rpc
  * **Option 2**: install [a namenode log4j Kafka appender](/docs/import-hdfs-auditLog.html) to stream hdfs audit log into Kafka automatically, and then run below command

        # install a log4j Kafka appender first
        $ hdfs dfs -cat /tmp/private

  You should see an alert for policy name “viewPrivate” in [Eagle web](http://localhost:9099/eagle-service) . Under Alerts page.

**Example 2** (HiveQueryLog): check sample policy “queryPhoneNumber” in [Eagle web](http://localhost:9099/eagle-service) by submitting a hive job

      $ su hive
      $ hive
      > set hive.execution.engine=mr;
      > use xademo;
      > select a.phone_number from customer_details a, call_detail_records b where a.phone_number=b.phone_number;

  You should see an alert for policy name “queryPhoneNumber” in [Eagle web](http://localhost:9099/eagle-service) . Under Alerts page.

<br/>

<br/>
