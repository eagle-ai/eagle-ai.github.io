---
layout: doc
title:  "Policy Management in Eagle" 
permalink: /docs/hdfspolicy.html

---

### Creating a HDFS Policy 
In this example we will go thru the steps for creating the following HDFS policy.

Policy: I would like to set an alert when a user is trying to delete a file 

##### Step 1
To create policy on HDFS

> Select HDFS as a source and HDFS Audit log as a stream

##### Step 2
> We support a variety of properties for match critera where users can set different values. 

* command = delete

* source = /tmp/private

##### Step 3

> Give a name to your policy and select de-duplication option if you need to avoid getting duplicates within a particular window. You have an option to configure email notifications for the alerts.


