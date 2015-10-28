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

### How to test ML

1. Download ML training and validation sample data from the [URL](http://eagle-in-one-box-awahl.ebayc3.com/ml-sample/)
    * user1.hdfs-audit.2015-10-11-00.txt and user1.hdfs-audit.2015-10-11-01.txt are used for training
    * validate file contains data points that you can try to test the models
2. Copy the files (downloaded in the previous step) into a location in sandbox
    * For example: /usr/hdp/current/eagle/lib/userprofile/data/
3. Modify <Eagle-home>/conf/sandbox-userprofile-scheduler.conf
    * update training-audit-path to set to the path for training data sample (the path you used for Step 1a)
    * update detection-audit-path to set to the path for validation (the path you used for Step 1b)
4. Run ML training program from eagle UI
5. Produce kafka data using the contents from validate file (Step 1b)
    * Run the command (assuming the eagle configuration uses kafka topic sandbox_hdfs_audit_log) -

		  ./kafka-console-producer.sh --broker-list sandbox.hortonworks.com:6667 --topic sandbox_hdfs_audit_log
    * Paste few lines of data from file validate onto kafka-console-producer
6. Check http://localhost:9099/eagle-service/#/dam/alertList for generated alerts

