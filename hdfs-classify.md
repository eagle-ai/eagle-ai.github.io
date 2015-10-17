---
layout: doc
title:  "HDFS Classification"
permalink: /docs/hdfs-classify.html
---

Data classification in Eagle provides the ability to classify HDFS and Hive data with different levels of sensitivity. For both HDFS and Hive, a user can add, delete, and browse the resources and add the sensitivity information.

For example: You can mark a particular folder/file which contains sensitivity data like address with a sensitive label. Once you have this information you can create policies using this label.

### Browsing HDFS
![HDFS classification](/images/docs/hdfsBrowse.png)

### Add Sensitivity

To add the sensitive info to files/directories, there are two ways.

* Option 1: Import json file/content

![HDFS classification](/images/docs/hdfsImport.png)
![HDFS classification](/images/docs/hdfsImport2.png)
![HDFS classification](/images/docs/hdfsImport3.png)

* Option 2: Label sensitivity files directly from the UI

![HDFS classification](/images/docs/hdfsMark.png)
![HDFS classification](/images/docs/hdfsMark2.png)
![HDFS classification](/images/docs/hdfsMark3.png)

### Delete Sensitivity

* Option 1

![HDFS classification](/images/docs/hdfsDelete.png)
![HDFS classification](/images/docs/hdfsDelete2.png)

* Option 2

![HDFS classification](/images/docs/hdfsRemove.png)
