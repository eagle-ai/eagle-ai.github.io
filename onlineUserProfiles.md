---
layout: doc
title:  "Online User Profiles"
permalink: /docs/onlineUserProfiles.html
---

This document will introduce how to start the online processing on user profiles. Assume Eagle has been installed and [Eagle service](http://sandbox.hortonworks.com:9099/eagle-service)
is started.

### Steps

Step 1: Start Spark if not started
![Start Spark](/images/docs/startSpark.png)

Step 2: submit userProfiles topology if it's not on [topology UI](http://sandbox.hortonworks.com:8744)

    bin/eagle-topology.sh --main eagle.security.userprofile.UserProfileDetectionMain --config conf/sandbox-userprofile-topology.conf --topology userprofile-topology start

Step 3: check the topology on [topology UI](http://sandbox.hortonworks.com:8744)


