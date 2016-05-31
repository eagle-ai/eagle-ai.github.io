---
layout: doc
title:  "Quick Start" 
permalink: /docs/quick-start.html
---

Guide To Install Eagle On Hortonworks sandbox. 

* Prerequisite
* Download + Patch + Build
* Setup Hadoop Environment.
* Install Eagle.
* Demo
<br/>

### **Prerequisite**
* To install Eagle on a sandbox, you need to run a HDP sandbox image in a virtual machine (8GB memory recommended).
	1. Get Virtual Box or VMware [Virtualization environment](http://hortonworks.com/products/hortonworks-sandbox/#install)
	2. Get [Hortonworks Sandbox v 2.2.4](http://hortonworks.com/products/hortonworks-sandbox/#archive)
* JDK 1.7  
* NPM (On MAC OS try "brew install node") 	
<br/>

### **Download + Patch + Build**
* Download latest Eagle source released From Apache [[Tar]](http://www-us.apache.org/dist/incubator/eagle/apache-eagle-0.3.0-incubating/apache-eagle-0.3.0-incubating-src.tar.gz) , [[MD5]](http://www-us.apache.org/dist/incubator/eagle/apache-eagle-0.3.0-incubating/apache-eagle-0.3.0-incubating-src.tar.gz.md5) 
* Build manually with [Apache Maven](https://maven.apache.org/):

	  $ tar -zxvf apache-eagle-0.3.0-incubating-src.tar.gz
	  $ cd incubator-eagle-release-0.3.0-rc3  
	  $ curl -O https://patch-diff.githubusercontent.com/raw/apache/incubator-eagle/pull/180.patch
	  $ git apply 180.patch
	  $ mvn clean package -DskipTests

	After building successfully, you will get tarball under `eagle-assembly/target/` named as `eagle-0.3.0-incubating-bin.tar.gz`
<br/>

### **Setup Hadoop Environment**
1. Launch Ambari to manage the Hadoop environment
   * Enable Ambari in sandbox [http://127.0.0.1:8000](http://127.0.0.1:8000) (Click on Enable Button)
   * Login to Ambari UI [http://127.0.0.1:8080](http://127.0.0.1:8080) with username and password as "admin"
2. Grant root as HBase superuser via Ambari
![add superuser](/images/docs/hbase-superuser.png)
3. Start HBase,Storm & Kafka from Ambari. Showing Storm as an example below. 
![Restart Services](/images/docs/start-storm.png "Services")
4. If the NAT network is used in a virtual machine, its required to add port 9099 to "Port Forwarding"
  ![Port Forwarding](/images/docs/eagle-service.png)
<br/>


### **Install Eagle**
    
     $ scp -P 2222  eagle-assembly/target/eagle-0.3.0-incubating-bin.tar.gz root@127.0.0.1:/root/
     $ ssh root@127.0.0.1 -p 2222 (password is hadoop)
     $ tar -zxvf eagle-0.3.0-incubating-bin.tar.gz
     $ mv eagle-0.3.0-incubating eagle
     $ mv eagle /usr/hdp/current/
     $ cd /usr/hdp/current/eagle
     $ examples/eagle-sandbox-starter.sh

<br/>

### **Demos**
* Login to Eagle UI [http://localhost:9099/eagle-service/](http://localhost:9099/eagle-service/) using username and password as "admin" and "secret"
* [HDFS & Hive](/docs/hdfs-hive-monitoring.html)
* [JMX Metric Monitoring](/docs/jmx-metric-monitoring.html)
<br/>
