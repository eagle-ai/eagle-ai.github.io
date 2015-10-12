---
layout: doc
title:  "How to import log data into Kafka"
permalink: /docs/importHDFSAuditLog.html
---

As Eagle consumes the data via Kafka topics in some applications, such as HDFS audit log monitoring, a user needs to populate its data into a Kafka topic.
There are two ways to do that. The first one is Logstash, which naturally supports Kafka as the output plugin; the second one is to
install a namenode log4j Kafka appender. In the following part, we take HDFS audit log as an example and give more details.

### **Logstash-kafka**

Step 1: Install Logstash-kafka

* For Logstash 1.5.x, logstash-kafka has been intergrated into [logstash-input-kafka](https://github.com/logstash-plugins/logstash-input-kafka) and [logstash-output-kafka](https://github.com/logstash-plugins/logstash-output-kafka),
and released with the 1.5 version of Logstash. So you can directly use it.

* For Logstash 1.4.x, a user should install [logstash-kafka](https://github.com/joekiller/logstash-kafka) firstly. Notice that this version **does not support partition\_key\_format**.

Step 2: Create a logstash configuration file under ${LOGSTASH_HOME}/conf. Here is a sample.

        input {
            file {
                type => "hdp-nn-audit"
                path => "/path/to/audit.log"
                start_position => end
                sincedb_path => "/var/log/logstash/"
             }
        }

        filter{
            if [type] == "hdp-nn-audit" {
        	   grok {
        	       match => ["message", "ugi=(?<user>([\w\d\-]+))@|ugi=(?<user>([\w\d\-]+))/[\w\d\-.]+@|ugi=(?<user>([\w\d.\-_]+))[\s(]+"]
        	   }
            }
        }

        output {
            if [type] == "hdp-nn-audit" {
                kafka {
                    codec => plain {
                        format => "%{message}"
                    }
                    broker_list => "localhost:9092"
                    topic_id => "hdfs_audit_log"
                    request_required_acks => 0
                    request_timeout_ms => 10000
                    producer_type => "async"
                    message_send_max_retries => 3
                    retry_backoff_ms => 100
                    queue_buffering_max_ms => 5000
                    queue_enqueue_timeout_ms => 5000
                    batch_num_messages => 200
                    send_buffer_bytes => 102400
                    client_id => "hdp-nn-audit"
                    partition_key_format => "%{user}"
                }
                # stdout { codec => rubydebug }
            }
        }

Step 3: Start Logstash

        cd path/to/logstash
        bin/logstash -f conf/sample.conf

Step 4: Validate it works

* Check name node is correctly started without log4j related exception.
* Check whether logs are flowing into the kafka topic specified by `topic_id`



### **Log4j Kafka Appender**

Notice that if you use ambari, such as in sandbox, you must following the following steps via Ambari UI. In addition, restarting namenode is required.

Step 1: Configure $HADOOP_CONF_DIR/log4j.properties, and add a log4j appender called "KAFKA_HDFS_AUDIT".

        log4j.appender.KAFKA_HDFS_AUDIT=eagle.log4j.kafka.KafkaLog4jAppender
        log4j.appender.KAFKA_HDFS_AUDIT.Topic=sandbox_hdfs_audit_log
        log4j.appender.KAFKA_HDFS_AUDIT.BrokerList=sandbox.hortonworks.com:6667
        log4j.appender.KAFKA_HDFS_AUDIT.KeyClass=eagle.log4j.kafka.hadoop.AuditLogKeyer
        log4j.appender.KAFKA_HDFS_AUDIT.Layout=org.apache.log4j.PatternLayout
        log4j.appender.KAFKA_HDFS_AUDIT.Layout.ConversionPattern=%d{ISO8601} %p %c{2}: %m%n
        log4j.appender.KAFKA_HDFS_AUDIT.ProducerType=sync
        #log4j.appender.KAFKA_HDFS_AUDIT.BatchSize=1
        #log4j.appender.KAFKA_HDFS_AUDIT.QueueSize=1

Step 2: Edit $HADOOP_CONF_DIR/hadoop-env.sh, and add the reference to KAFKA_HDFS_AUDIT to HADOOP_NAMENODE_OPTS.

        -Dhdfs.audit.logger=INFO,DRFAAUDIT,KAFKA_HDFS_AUDIT

Step 3: Add logstash kafka jars into Hadoop classpath by appending the following command to $HADOOP_CONF_DIR/hadoop-env.sh

        export HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/path/to/eagle/lib/log4jkafka/lib/*

Step 4: restart the namenode

Step 5: Validate it works

* Check name node is correctly started without log4j related exception.
* Check whether logs are flowing into Kafka with Kafka command line `bin/kafka-console-consumer.sh`









