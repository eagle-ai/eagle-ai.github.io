---
layout: doc
title:  "Overview" 
permalink: /docs/index.html
---

### WHAT IS EAGLE

Eagle is an Open Source Monitoring solution for Hadoop to instantly detect access to sensitive data, recognize attacks, malicious activities and block access in real time.

### WHO USES EAGLE

Eagle data activity monitoring is currently deployed at eBay for monitoring the data access activities in a 2500 node hadoop cluster with plans of extending it to other hadoop clusters covering 10,000 nodes by end of this year. 

We have wide range of policies to detect and prevent data loss, data copy to unsecured location, sensitive data access from unauthorized zones etc. The flexibility of creating policies in eagle allows us to expand further and add more complex policies.

### WHAT'S NEXT

At eBay, Eagle framework is also used extensively to monitor health of data nodes, hadoop applications, hadoop core services and the entire hadoop cluster health for the past 2 years.


Below are some of the features we are currently working on:

* Machine learning models for hive and hbase data access features and various dimensions.

* Extensible API for integration with tools like splunk for reporting, dataguise for data classification.

* We will be releasing a new module for hadoop cluster monitoring in the next version of eagle.
	* Hbase service monitoring 
	* Hadoop job performance monitoring 
	* Hadoop node monitoring


### ROADMAP
 ![Roadmap](/images/docs/Roadmap.png "Eagle Roadmap")