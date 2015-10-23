---
layout: doc
title:  "User Profile Tutorial"
permalink: /docs/tutorial/userprofile.html
---
This document will introduce how to start the online processing on user profiles. Assume Eagle has been installed and [Eagle service](http://sandbox.hortonworks.com:9099/eagle-service)
is started.

### User Profile Offline Training

* **Step 1**: Start Spark if not started
![Start Spark](/images/docs/start-spark.png)

* **Step 2**: start offline scheduler

	* Option 1: command line

	      $ cd <eagle-home>/bin
	      $ bin/eagle-userprofile-scheduler.sh --site sandbox start

	* Option 2: start via Ambari
	![Click "ops"](/images/docs/offline-userprofile.png)

* **Step 3**: generate a model

	![Click "ops"](/images/docs/userprofile1.png)
	![Click "Update Now"](/images/docs/userprofile2.png)
	![Click "Confirm"](/images/docs/userprofile3.png)
	![Check](/images/docs/userprofile4.png)

### User Profile Online Detection

Two options to start the topology are provided.

* **Option 1**: command line

	submit userProfiles topology if it's not on [topology UI](http://sandbox.hortonworks.com:8744)

      $ bin/eagle-topology.sh --main eagle.security.userprofile.UserProfileDetectionMain --config conf/sandbox-userprofile-topology.conf start

* **Option 2**: Ambari
	
	![Online userProfiles](/images/docs/online-userprofile.png)

