---
layout: doc
title:  "Deploy Environment"
permalink: /docs/deployment-env.html
---

### Setup Environment

Apache Eagle (incubating, called Eagle in the follwing) as an analytics solution for identifying security and performance issues instantly, relies on streaming platform `storm` + `Kafka` to meet the realtime criteria, and persistence storage to store metadata and some metrics. As for the persistence storage, it supports three types of database: `HBASE`, `Derby`, and `Mysql`

To run monitoring applications, Eagle requires the following dependencies.

* For Streaming platform Dependencies

	* Storm: 0.9.3 or later
	* Kafka: 0.8.x or later
	* Java: 1.7.x
	* NPM (On MAC OS try "brew install node") 	

* For Database Dependencies (Choose one of them)

	* HBase: 0.98 or later
		* Hadoop: 2.6.x is required
	* Mysql
		* Mysql installation is required
	* Derby
		* No installation is required 
		
### Setup Cluster in Sandbox
To make thing easier you can try Eagle with an **all-in-one** sandbox VM, like [HDP sandbox](http://hortonworks.com/downloads/#sandbox)(HDP 2.2.4 is recommended). Next we will go with Hortonworks Sandbox 2.2.4 to setup a minimal requirement cluster with Storm and Kafka. 

1. Launch Ambari 
   * Enable Ambari in sandbox http://127.0.0.1:8000 (Click on Enable Button)
   * Login to Ambari UI http://127.0.0.1:8080/ with user:admin and password:admin

2. Start Storm and Kafka via Ambari. Showing Storm as an example below.
![Restart Services](/images/docs/start-storm.png "Services")

3. (Optional) Start HBase via Ambari with root as HBase superuser
![add superuser](/images/docs/hbase-superuser.png)
![add superuser](/images/docs/hbase-superuser2.png)

4. Add Eagle service port. If the NAT network is used in a virtual machine, add port 9099 to "Port Forwarding"
  ![Port Forwarding](/images/docs/eagle-service.png)





