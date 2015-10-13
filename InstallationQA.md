---
layout: doc
title:  "Installation Q & A"
permalink: /docs/installationQA.html
---

Q1. Not able to access storm worker log via browser

A1: add the following line in host machine's hosts file

      127.0.0.1 sandbox.hortonworks.com

Q2. Not able send data into kafka using kafka console producer: /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list localhost:6667 --topic sandbox_hdfs_audit_log

A2: kafka broker are binding to host sandbox.hortonworks.com

       /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list sandbox.hortonworks.com:6667 --topic sandbox_hdfs_audit_log
       cat /etc/hosts
       127.0.0.1 localhost.localdomain localhost
       10.0.2.15 sandbox.hortonworks.com sandbox ambari.hortonworks.com

Q3. Cannot visit eagle service url http://localhost:9099 through the browser

A3: If your network is NAT, then you need to add forwarding port 9099 for eagle service. Or you should check if HBase is alive

