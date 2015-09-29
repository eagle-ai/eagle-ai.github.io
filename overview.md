---
layout: doc
title:  "Overview" 
permalink: /docs/index.html
---

## What is Eagle

Eagle is an Open Source Monitoring solution for Hadoop to instantly detect access to sensitive data, recognize attacks, malicious activities and block access in real time.

## Who uses Eagle

Eagle data activity monitoring is currently being used in production at eBay for monitoring the data access activities in a 2500 node hadoop cluster with plans of extending it to other hadoop clusters covering 10,000 nodes by end of this year. 

We have wide range of policies to stop data loss, data copy to unsecured location, sensitive data access from unauthorized zones etc. The flexibility of creating policies in eagle allows us to expand further and add more complex policies.

## What's next 

At eBay Eagle framework is also used extensively to monitor health of data nodes, hadoop applications, hadoop core services and the entire hadoop cluster health for the past 2 years.

Below are some of the features we are working on:

* Machine learning models for hive and hbase data access features and various dimensions.
* Extensible API for integration with tools like splunk for reporting, dataguise for data classification.
* We will be releasing a new module for hadoop cluster monitoring in the next version of eagle.
	* Hbase service monitoring 
	* Hadoop job performance monitoring 
	* Hadoop node monitoring