---
layout: doc
title:  "Site Management"
permalink: /docs/setup.html
---

Eagle identifies different data platforms as different sites, such as sandbox, datacenter1, datacenter2. In each site,
a user can add at most three data sources as the monitoring targets: hdfsAuditLog, HiveQueryLog, and userProfiles. For
each data source, a connection configuration is required.

This document has two parts. The first part is about how to add a new site; the second part shows how to add the configuration for
each data source.

**Notice**: by default, Site "sandbox" is provided with three configured data sources.

### **Part 1: Add Site**

The following is an example which creates a new site "Demo", and add two data sources as its monitoring targets.
![setup a site](/images/docs/new-site.png)

### **Part 2: Add Configuration**

Different sites need different configuration. Here are example configurations for sandbox
![hdfs setup](/images/docs/hdfs-setup.png)
![hdfs setup](/images/docs/hive-setup.png)
![hdfs setup](/images/docs/userprofile-setup.png)