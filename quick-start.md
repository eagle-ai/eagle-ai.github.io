---
layout: doc
title:  "Quick Start" 
permalink: /docs/quick-start.html
---

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

### Step 4: Test policy alert by making some operations on HDFS

`TODO`
<br/>

### Step5: Analysis alert

`TODO`
<br/>
