---
layout: doc
title:  "Data Classification Tutorial" 
permalink: /docs/tutorial/classification.html
---

Eagle Data classification provides the ability to classify HDFS and Hive data with different levels of sensitivity.
For both HDFS and Hive, a user can browse the resources and add/remove the sensitivity information.

### HDFS Classification

The document has two parts. The first part is about how to add/remove sensitivity to files/directories; the second part shows the application
of sensitivity in policy definition. Showing HDFS as an example.

> **WARNING**: please mark sure the site (on the upper right corner) have been correctly configured on [setup page](/docs/setup.html) AT FIRST.

#### **Part 1: Sensitivity Edit**

* add the sensitive mark to files/directories.

    * **Option 1**: Import json file/content

    ![HDFS classification](/images/docs/hdfs-import1.png)
    ![HDFS classification](/images/docs/hdfs-import2.png)
    ![HDFS classification](/images/docs/hdfs-import3.png)

    * **Option 2**: Label sensitivity files directly (**recommended**)

    ![HDFS classification](/images/docs/hdfs-mark1.png)
    ![HDFS classification](/images/docs/hdfs-mark2.png)
    ![HDFS classification](/images/docs/hdfs-mark3.png)

* remove sensitive mark on files/directories

    * **Option 1**

    ![HDFS classification](/images/docs/hdfs-delete1.png)
    ![HDFS classification](/images/docs/hdfs-delete2.png)

    * **Option 2**

    ![HDFS classification](/images/docs/hdfs-remove.png)

#### **Part 2: Sensitivity Usage in Policy Definition**

For example: You can mark a particular folder/file which contains sensitivity data like address with a sensitive label. Once you have this information you can create policies using this label.
the following policy monitors all the operations to resources with sensitivity type "PRIVATE".
![sensitivity type policy](/images/docs/sensitivity-policy.png)

### Hive Classification
Data classification in Eagle provides the ability to classify HDFS and Hive data with different levels of sensitivity. For both HDFS and Hive, a user can add, delete, and browse the resources and add the sensitivity information.

For example: You can label a particular table, column or columns which contains sensitivity data like address, phone number with sensitivity definitions. Once you have this information you can create policies using this label.


#### Browsing Hive Sensitivity 

![Hive classification](/images/docs/hiveBrowse.png)

#### Add Sensitivity

To add the sensitive info to files/directories, there are two ways.

* **Option 1**: Import json file/content

	![Hive classification](/images/docs/hive-import.png)
	![Hive classification](/images/docs/hive-import2.png)
	![Hive classification](/images/docs/hive-import3.png)

* **Option 2**: Classify from the UI

	![Hive classification](/images/docs/hiveMark.png)
	![Hive classification](/images/docs/hiveMark2.png)
	![Hive classification](/images/docs/hiveMark3.png)

#### Delete Sensitivity

* Option 1

![Hive classification](/images/docs/hiveDelete.png)
![Hive classification](/images/docs/hiveDelete2.png)

* Option 2

![Hive classification](/images/docs/hiveRemove.png)