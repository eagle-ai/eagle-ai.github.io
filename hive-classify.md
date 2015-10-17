---
layout: doc
title:  "Hive Classification"
permalink: /docs/hive-classify.html
---


Data classification in Eagle provides the ability to classify HDFS and Hive data with different levels of sensitivity. For both HDFS and Hive, a user can add, delete, and browse the resources and add the sensitivity information.

For example: You can label a particular table, column or columns which contains sensitivity data like address, phone number with sensitivity definitions. Once you have this information you can create policies using this label.


#### Browsing Hive Sensitivity 

![Hive classification](/images/docs/hiveBrowse.png)

#### Add Sensitivity

To add the sensitive info to files/directories, there are two ways.

* **Option 1**: Import json file/content

	![Hive classification](/images/docs/hiveImport.png)
	![Hive classification](/images/docs/hiveImport2.png)
	![Hive classification](/images/docs/hiveImport3.png)

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