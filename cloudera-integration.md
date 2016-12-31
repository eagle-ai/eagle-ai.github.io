---
layout: doc
title:  "Cloudera Integration"
permalink: /docs/cloudera-integration.html
---

*Since Eagle 0.4.0*

This tutorial is about how to make Apache Eagle work on Cloudera cluster.

### Prerequisites

To get maprFSAuditLog monitoring started, we need to:

* Zookeeper (installed through Cloudera Manager)
* Kafka (installed through Cloudera Manager)
* Storm (0.9.x or 0.10.x, installed manually)
* Logstash (2.X, installed manually on NameNode)


### Kafka
#### Configuration
There are two points needed to be mentioned for KAFKA: 

* Open Cloudera, and go to "kafka" configuration, set ``“zookeeper Root”`` to ``“/”`` .

* If Kafka cannot be started successfully, check kafka’s log. If stack trace shows: ``“java.lang.OutOfMemoryError: Java heap space”``. Increase heap size by setting 
~~~
                  export KAFKA_HEAP_OPTS="-Xmx2G -Xms2G"
~~~
  in ``/bin/kafka-server-start.sh``.


#### Verification
- Step1: create a kafka topic (here I created a topic called “test”)
~~~
bin/kafka-topics.sh --create --zookeeper 127.0.0.1:2181 --replication-factor 1 --partitions 1 --topic test
~~~

- Step2: check if topic has been created successfully.
~~~
bin/kafka-topics.sh --list --zookeeper 127.0.0.1:2181
~~~
You should see the topic created by you.

- Step3: open two terminals, start “producer” and “consumer” separately.
~~~
/usr/bin/kafka-console-producer --broker-list hostname:9092 --topic test
/usr/bin/kafka-console-consumer --zookeeper hostname:2181 --topic test
~~~

- Step4: type in some message in producer
If consumer cannot receive the message, check the configuration and logs, otherwise kafka is all set.


### Logstash
#### Installation
You can follow this [link](https://www.elastic.co/downloads/logstash) to install logstash on your machine:

Or you can install it through "yum" if you are using centos:

- download and install the public signing key:
~~~
rpm --import  https://packages.elastic.co/GPG-KEY-elasticsearch
~~~

- Add the following lines in ``/etc/yum.repos.d/`` directory in a file with a ``.repo`` suffix, for example ``logstash.repo``.
~~~
[logstash-2.3]
name=Logstash repository for 2.3.x packages
baseurl=https://packages.elastic.co/logstash/2.3/centos
gpgcheck=1
gpgkey=https://packages.elastic.co/GPG-KEY-elasticsearch
enabled=1
~~~

- Then install it using yum: 
~~~
yum install logstash
~~~

#### Create conf file
Follow this [link](https://github.com/apache/incubator-eagle/blob/branch-0.4/eagle-assembly/src/main/docs/logstash-kafka-conf.md) to create configuration file in logstash for Apache Eagle.

#### Start logstash
~~~
bin/logstash -f conf/first-pipeline.conf
~~~
#### Verification
Open a terminal and start a kafka consumer to see if it can receive the messages sent by logstash, if there is no message, double check the configuration parameters in conf file. Otherwise logstash is all set.

### Apache Storm
As storm is not in Cloudera’s stack, we need to install Storm manually.

#### Installation
Follow this link to install storm on your cluster, the version you choose should be ``0.10.x`` or ``0.9.x`` release.

In ``/etc/profile``:, add this: 
~~~
export PATH=$PATH:/opt/apache-storm-0.10.1/bin/
~~~
save the profile and then type:
~~~
source /etc/profile 
~~~
to make it work.

#### Configuration

In ``storm/conf/storm.yaml``, change the hostname to your own host.

#### Start Apache Storm
In Termial, type:
~~~
$: storm nimbus
$: storm supervisor
$: storm UI
~~~
#### Verification

Open storm UI in your browser, default URL is : ``http://hostname:8080/index.html``.

### Apache Eagle
Finally, we come to Apache Eagle. Most of its configuration should be the same as it is in Hortonworks, we only need to modify one thing in ``“apache-eagle-0.4.0-incubating/bin/ eagle-topology.sh”``, line 102:
~~~			
			storm_ui=http://localhost:8080
~~~
If you are not using the default port number, change this to your own storm ui’s url, then you are all set.
