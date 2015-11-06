---
layout: doc
title:  "Site Management"
permalink: /docs/tutorial/setup.html
---

Eagle identifies different Hadoop environments as different sites, such as sandbox, datacenter1, datacenter2. In each site,
a user can add different data sources as the monitoring targets. For each data source, a connection configuration is required.

This page shows how to setup a site completely. It has two parts. The first part is about how to add a new site on the web;
the second part shows how to configure each data source in this site.

> **Notice**: the following configuration part, we uses Hortonworks sandbox as examples

### **Part 1: Add Site**

The following is an example which creates a new site "Demo", and add two data sources as its monitoring targets.
![setup a site](/images/docs/new-site.png)

### **Part 2: Add Configuration**

After create a new site, we need to configure the connection string for each data source in this site.

* HDFS Audit Log

        {"hdfsEndpoint":"hdfs://sandbox.hortonworks.com:8020"}

  ![hdfs setup](/images/docs/hdfs-setup.png)

* Hive Query Log

        {
          "accessType": "metastoredb_jdbc",
          "password": "hive",
          "user": "hive",
          "jdbcDriverClassName": "com.mysql.jdbc.Driver",
          "jdbcUrl": "jdbc:mysql://sandbox.hortonworks.com/hive?createDatabaseIfNotExist=true"
        }

![hdfs setup](/images/docs/hive-setup.png)

* UserProfile

        {
          "features": "getfileinfo,open,listStatus,setTimes,setPermission,rename,mkdirs,create,setReplication,contentSummary,delete,setOwner,fsck"
        }

![hdfs setup](/images/docs/userprofile-setup.png)