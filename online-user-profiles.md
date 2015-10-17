---
layout: doc
title:  "Online User Profiles"
permalink: /docs/online-user-profiles.html
---

This document will introduce how to start the online processing on user profiles. Assume Eagle has been installed and [Eagle service](http://sandbox.hortonworks.com:9099/eagle-service)
is started. Two options to start the topology are provided.

* **Option 1**: command line

	submit userProfiles topology if it's not on [topology UI](http://sandbox.hortonworks.com:8744)

      $ bin/eagle-topology.sh --main eagle.security.userprofile.UserProfileDetectionMain --config conf/sandbox-userprofile-topology.conf start

* **Option 2**: Ambari
	
	![Online userProfiles](/images/docs/onlineUserProfile.png)


