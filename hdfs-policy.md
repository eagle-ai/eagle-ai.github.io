---
layout: doc
title:  "Policy Management in Eagle" 
permalink: /docs/hdfs-policy.html

---

### Creating a Policy for HDFS 
In this example we will go thru the steps for creating the following HDFS policy.

> Example Policy: Create a policy to alert when a user is trying to delete a file with sensitive data

* **Step 1**: Select Source as HDFS and Stream as HDFS Audit Log

	![HDFS Policies](/images/docs/hdfs-policy1.png)

* **Step 2**: Eagle support a variety of properties for match critera where users can set different values. Eagle also supports window functions to extend policies with time functions.

	  command = delete 
	  (Eagle currently supports the following commands open, delete, copy, append, copy from local, get, move, mkdir, create, list, change permissions)
		
	  source = /tmp/private 
	  (Eagle supports wildcarding for property values for example /tmp/*)

	  sensitivity type = Address
	  (Eagle supports classifying data in HDFS with different sensitivity types. Users can use these sensitivity types to create policies)

	![HDFS Policies](/images/docs/hdfs-policy2.png)

* **Step 3**: Name your policy and select de-duplication options if you need to avoid getting duplicate alerts within a particular time window. You have an option to configure email notifications for the alerts.

	![HDFS Policies](/images/docs/hdfs-policy3.png)