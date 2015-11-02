---
layout: doc
title:  "Quick Start" 
permalink: /docs/quick-start.html
---

This is a tutorial-style guide for users to have a quick image of Eagle. The main content are

* Step 1: Download/Build tarball
* Step 2: Install Eagle
* Step 3: Define policy with Eagle web
* Step 4: Test policy and check alerting

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

Learn more about how to define policy, please refer to tutorial: [Policy Management](/docs/tutorial/policy.html)
<br/>

### Step 4: Test policy and check alerting

We show two examples to validate the sample policies defined in sandbox.

**Example 1** (HDFSAuditLog): validate sample policy “viewPrivate” on [Eagle web](http://localhost:9099/eagle-service) by running a HDFS command

      $ hdfs dfs -cat /tmp/private

  You should see an alert for policy name “viewPrivate” in [Eagle web](http://localhost:9099/eagle-service) . Under Alerts page.

**Example 2** (HiveQueryLog): validate sample policy “queryPhoneNumber” in [Eagle web](http://localhost:9099/eagle-service) by submitting a hive job

      $ su hive
      $ hive
      > set hive.execution.engine=mr;
      > use xademo;
      > select a.phone_number from customer_details a, call_detail_records b where a.phone_number=b.phone_number;

  You should see an alert for policy name “queryPhoneNumber” in [Eagle web](http://localhost:9099/eagle-service) . Under Alerts page.

<br/>

<br/>
